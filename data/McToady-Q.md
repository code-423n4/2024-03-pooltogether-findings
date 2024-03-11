| Severity | Title                                                                                                                                                                                                                    |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| L-1      | No guarantee that PrizeVault deployed from PrizeVaultFactory uses a legitimate PrizePool contract                                                                                                                        |
| L-2      | PrizeVault constructor does not assert `yieldBuffer_` is non-zero which could cause issues for PrizeVault instances not deployed by the PrizeVaultFactory                                                                |
| L-3      | Winners can gas grief the `Claimable::claimer` address by ensuring their `beforeClaimPrize` and `afterClaimPrize` hooks both use the maximum 150,000 gas                                                                 |

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

# [L-3] Winners can gas grief the `Claimable::claimer` address by ensuring their `beforeClaimPrize` and `afterClaimPrize` hooks both use the maximum 150,000 gas

[Instance One](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L76)

Setting gas limits on the users hooks is a smart decision by the protocol team however, they should consider allowing only a maximum of `HOOK_GAS` is used between both the `beforeClaimPrize` and `afterClaimPrize` hooks.

Below is an example of gas price as of today (2024/03/11, with a conservative gas price estimate) in USDC of only the hooks if a user were to use the maximum 300,000 gas for each of their hooks on Ethereum mainnet:
![gas price](https://i.imgur.com/SFQQNMh.png)

This means winners would be able to gas grief `claimer` causing them to spend at minimum $60 to process call `claimPrize` on the winners behalf. Reducing this to a total 150,000 gas for both hooks seems like a reasonable middle ground.
