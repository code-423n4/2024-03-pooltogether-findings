| Severity | Title                                                                                                                                                                                                                    |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| L-1      | No guarantee that PrizeVault deployed from PrizeVaultFactory uses a legitimate PrizePool contract                                                                                                                        |
| L-2      | PrizeVault constructor does not assert `yieldBuffer_` is non-zero which could cause issues for PrizeVault instances not deployed by the PrizeVaultFactory                                                                |
| L-3      | A malicious `PrizeVault::owner` could frontrun calls to `PrizeVault::transferTokensOut` raising the `yieldFeePercentage` causing the liquidation attempt to revert or blindsiding users with an unsuspected fee increase |
| L-4      | Winners can gas grief the `Claimable::claimer` address by ensuring their `beforeClaimPrize` and `afterClaimPrize` hooks both use the maximum 150,000 gas                                                                 |

# [L-1] No guarantee that PrizeVault deployed from PrizeVaultFactory uses a legitimate PrizePool contract

[Instance One](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L92)

**Description**

The PrizeVaultFactory is trustless allowing anyone to deploy their on PrizeVault instance. One of the arguments to `deployVault` requires the user sets the `_prizePool` address, however there is no check that this is a legitimate prizepool contract.

**Impact**

One example of how this could be used to steal users funds:

1. Attacker deploys an otherwise standard PrizeVault with a malicious PrizePool contract.
2. Attacks PrizePool contract always selects a "random" winner that is an address controlled by the attacker.
3. Depositors to the vault are unknowingly giving up all their yield to the attacker with no chance of winning.

**Mitigation**

Given the `allVaults` array and `deployedVaults` mapping it's reasonable to consider PrizeVaultFactory would be considered something of a source of truth by users and expect that any PrizeVaults deployed by this contract to be legitimate. To ensure this is the case the PrizeVaultFactory should consider keeping a mapping of legitimate PrizePool implementations and ensuring that the address passed to `_prizePool` is one of these addresses.

```diff
-   contract PrizeVaultFactory {
+   contract PrizeVaultFactory is Ownable {

+       mapping(address prizePool => bool isLegitimate) public isPrizePool;

        function deployVault(
            string memory _name,
            string memory _symbol,
            IERC4626 _yieldVault,
            PrizePool _prizePool,
            address _claimer,
            address _yieldFeeRecipient,
            uint32 _yieldFeePercentage,
            address _owner
        ) external returns (PrizeVault) {
+           require(isPrizePool[address(_prizePool)], "Not Legit PrizePool");
            // -- snip --
        }

+       function setPrizePool(address _prizePool, bool _isLegitimate) external onlyOwner {
+           isPrizePool[_prizePool] = _isLegitimate;
+       }

    }
```

# [L-2] PrizeVault constructor does not assert `yieldBuffer_` is non-zero which could cause issues for PrizeVault instances not deployed by the PrizeVaultFactory

[Instance One](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L289)

The documentation in the PrizeVault contract goes into a great amount of detail outlining the importance of the yield buffer in avoiding the key invariant (`totalAssets >= totalSuppy` from being broken. For contracts deployed by the PrizeVaultFactory a constant value (1e5) is set as the `yieldBuffer` for all contracts. However if an instance of PrizeVault is deployed not using the factory contract there are no checks in PrizeVault's constructor stopping the `yieldBuffer` from being set to zero.

**Mitigation**

The protocol should consider adding a check to PrizeVaults constructor that `yieldBuffer` is set to some minimum value to limit the chances of a vault instance causing a loss of user funds.

# [L-3] A malicious `PrizeVault::owner` could frontrun calls to `PrizeVault::transferTokensOut` raising the `yieldFeePercentage` causing the liquidation attempt to revert or blindsiding users with an unsuspected fee increase

[Instance One](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L753)

**Impact**

Assuming the liquidator who calls `LiquidationPair::swapExactAmountOut` attemps to swap the maximum liquidatable amount from the PrizeVault it would be possible for the PrizeVault's owner to sandwich their transaction, first raising the `yieldFeePercentage` (thus causing the transaction to revert with a `LiquidationExceedsAvailable` error), then lowering the `yieldFeePercentage` back to normal after the transaction fails. On the other hand, if the liquidator attempts to liquidate less than the maximum liquidatable amount this gives the PrizeVault owner the opportunity to raise fees for real and take a larger cut than the depositers are expecting.

**Proof of Concept**

## Liquidator attempts to liquidate max and tx reverts

Add the following test to PrizeVault.t.sol to confirm this:

```solidity
    function test_YieldFeeBalanceFeeFrontrunRevert() public {
        vault.setYieldFeePercentage(1e7); // 1%
        vault.setYieldFeeRecipient(bob);
        assertEq(vault.totalDebt(), 0);

        // Make an initial deposit
        underlyingAsset.mint(alice, 1e18);
        vm.startPrank(alice);
        underlyingAsset.approve(address(vault), 1e18);
        vault.deposit(1e18, alice);
        vm.stopPrank();

        // Add funds to liquidiate
        underlyingAsset.mint(address(vault), 1e18);
        vault.setLiquidationPair(address(this));
        // Calculate how much to liquidiate
        uint256 maxLiquidation = vault.liquidatableBalanceOf(address(underlyingAsset));
        // Attempt to claim max liquidable amount
        uint256 amountOut = maxLiquidation;

        // Bump yieldFeePercentage before liquidator can call transferTokensOut
        vault.setYieldFeePercentage(1e8);

        // Attempt transfer tokens out
        vault.transferTokensOut(address(0), bob, address(underlyingAsset), amountOut);
    }
```

## Liquidiate attempts to liquidate less than max and owner gets extra fees

Add the follwing test to PrizeVault.t.sol to confirm this:

```solidity
    function test_YieldFeeBalanceFeeFrontrunFeeSteal() public {
        vault.setYieldFeePercentage(1e7); // 1%
        vault.setYieldFeeRecipient(bob);
        assertEq(vault.totalDebt(), 0);

        // Make an initial deposit
        underlyingAsset.mint(alice, 1e18);
        vm.startPrank(alice);
        underlyingAsset.approve(address(vault), 1e18);
        vault.deposit(1e18, alice);
        vm.stopPrank();

        // Add funds to liquidiate
        underlyingAsset.mint(address(vault), 1e18);
        vault.setLiquidationPair(address(this));
        // Calculate how much to liquidiate
        uint256 maxLiquidation = vault.liquidatableBalanceOf(address(underlyingAsset));
        // Liquidates less than max
        uint256 amountOut = maxLiquidation / 2;

        // Bump yieldFeePercentage before liquidator can call transferTokensOut
        vault.setYieldFeePercentage(1e8);

        // Attempt transfer tokens out
        vault.transferTokensOut(address(0), bob, address(underlyingAsset), amountOut);
    }
```

The tests provide the following results:

```solidity
    [PASS] test_YieldFeeBalanceFeeFrontrunFeeSteal() (gas: 438292)
    [FAIL. Reason: LiquidationExceedsAvailable(1099999999998900000 [1.099e18], 999999999999000000 [9.999e17])] test_YieldFeeBalanceFeeFrontrunRevert() (gas: 467793)
    Test result: FAILED. 1 passed; 1 failed; 0 skipped; finished in 2.43ms
```

**Recommended Mitigation**

It would be preferable if the PrizeVault owner did not have the power to change the `yieldFeePercentage` while a lottery round is ongoing. Removing this ability would deny this griefing opportunity but also ensure that users aren't blind sided by the owner raising fees shortly before the yield generated is liquidated.

# [L-4] Winners can gas grief the `Claimable::claimer` address by ensuring their `beforeClaimPrize` and `afterClaimPrize` hooks both use the maximum 150,000 gas

[Instance One](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L76)

Setting gas limits on the users hooks is a smart decision by the protocol team however, they should consider allowing only a maximum of `HOOK_GAS` is used between both the `beforeClaimPrize` and `afterClaimPrize` hooks.

Below is an example of gas price as of today (2024/03/11, with a conservative gas price estimate) in USDC of only the hooks if a user were to use the maximum 300,000 gas for each of their hooks on Ethereum mainnet:
![gas price](https://i.imgur.com/SFQQNMh.png)

This means winners would be able to gas grief `claimer` causing them to spend at minimum $60 to process call `claimPrize` on the winners behalf. Reducing this to a total 150,000 gas for both hooks seems like a reasonable middle ground.
