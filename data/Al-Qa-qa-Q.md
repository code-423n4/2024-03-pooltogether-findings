## Summary

|ID|Title|
|:-:|:---|
|[L-01](#l-01-checking-previous-approval-before-permit-is-done-with-strict-equality)|Checking previous approval before `permit()` is done with strict equality|
|[L-02](#l-02-unchecking-for-the-actual-assets-withdrawn-from-the-vault-can-prevent-users-from-withdrawing)|Unchecking for the actual assets withdrawn from the vault can prevent users from withdrawing|
|[L-03](#l-03-no-checking-for-breefy-vault-strategies-state-paused-or-not)|No checking for Breefy Vault strategies state (paused or not)|
|||
|[NC-01](#nc-01-assets-must-precede-the-shares-according-to-the-erc4626-standard)|Assets must precede the shares according to the ERC4626 standard|

---

## [L-01] Checking previous approval before `permit()` is done with strict equality

In `PrizeVault::depositWithPermit`, the check that is implemented by the protocol to overcome griefing users by frontrunning permit signing, is to check if the owner allowance equals or does not equal the assets being transferred.

[PrizeVault.sol#L539-L541](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L539-L541)
```solidity
    function depositWithPermit(... ) external returns (uint256) {
        ...

        // @audit strict check (do not check if the allowance exceeds required)
        if (_asset.allowance(_owner, address(this)) != _assets) {
            IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);
        }

        ...
    }
``` 

So if the user approval exceeds the amount he wanted to do, the permit still will occur.

- Let's say Bob approved 1000 tokens.
- Bob wants to transfer 500 tokens.
- Bob first the function `depositWithPermit()`.
- (1000 != 500), and Bob will have an allowance of 1500 tokens when he only needs 500.

## Recommended Mitigations
Allow permit if the allowance is Smaller than the required assets

```diff
    function depositWithPermit(... ) external returns (uint256) {
        ...

-       if (_asset.allowance(_owner, address(this)) != _assets) {
+       if (_asset.allowance(_owner, address(this)) < _assets) {
            IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);
        }

        ...
    }
```

---

## [L-02] Unchecking for the actual assets withdrawn from the vault can prevent users from withdrawing

When withdrawing assets from `PrizeVault`, the contract assumes that the requested assets for redeeming are the actual assets the contract (PrizeVault) received.

[PrizeVault.sol#L936](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L936)
```solidity
    function _withdraw(address _receiver, uint256 _assets) internal {
        ...
        if (_assets > _latentAssets) {
            ...
            // @audit no checking for the returned value (assets)
            yieldVault.redeem(_yieldVaultShares, address(this), address(this));
        }
        if (_receiver != address(this)) {
            _asset.transfer(_receiver, _assets);
        }
    }
```

Since the calculations are done by rounding Up asset value when getting shares in `previewWithdraw`, and then rounding Down shares when getting assets, there should not be any 1 wei rounding error.

This will not be the case for all `yieldVaults` supported by `PrizeVault`, where the amount can decrease if there is no enough balance in the vault for example, as that of `Beefy Vaults`.

[BIFI/vaults/BeefyWrapper.sol#L156-L158](https://github.com/beefyfinance/beefy-contracts/blob/master/contracts/BIFI/vaults/BeefyWrapper.sol#L156-L158)
```solidity
    function _withdraw( ... ) internal virtual override {
        ...

        uint balance = IERC20Upgradeable(asset()).balanceOf(address(this));
        if (assets > balance) {
            // @audit the assets requested for withdrawal will decrease
            assets = balance;
        }

        IERC20Upgradeable(asset()).safeTransfer(receiver, assets);

        emit Withdraw(caller, receiver, owner, assets, shares);
    }
```

And ofc if there is a fee on withdrawals, the amount will decrease but this is OOS.

## Recommended Mitigations

Check the returned value of assets transferred after withdrawing, and take suitable action if the value differs from the value requested. And take suitable action like emitting events to let Devs know what problem occurred when this user withdrew.

---

## [L-03] No checking for Breefy Vault strategies state (paused or not)

When depositing or minting into a vault, the check that is done by the `ERC4626` is checking the `maxDeposit` parameter.

This function `maxDeposit()` will return 0 if the Vault is in pause state like AAVE-v3 [link](https://github.com/timeless-fi/yield-daddy/blob/main/src/aave-v3/AaveV3ERC4626.sol#L161-L164).

But In case of breefy, when depositing The function go to the internal deposie function first

1. [BIFI/vaults/BeefyWrapper.sol#L117-L130](https://github.com/beefyfinance/beefy-contracts/blob/master/contracts/BIFI/vaults/BeefyWrapper.sol#L117-L130)
```solidity
    // BeefyWrapper.sol
    function _deposit( ... ) internal virtual override {
        ...
        // @audit Step 1
        IVault(vault).deposit(assets);

        ...
    }
```
2. [/BIFI/vaults/BeefyVaultV7.sol#L100-L115](https://github.com/beefyfinance/beefy-contracts/blob/master/contracts/BIFI/vaults/BeefyVaultV7.sol#L100-L115)
```solidity
    // BeefyVaultV7.sol
    function deposit(uint _amount) public nonReentrant {
        // @audit Step 2
        strategy.beforeDeposit();

        ...
    }
```
3. [BIFI/strategies/Aave/StrategyAaveSupplyOnlyOptimism.sol#L104-L109](https://github.com/beefyfinance/beefy-contracts/blob/master/contracts/BIFI/strategies/Aave/StrategyAaveSupplyOnlyOptimism.sol#L104-L109)
```solidity
    // @audit Step 3
    // StrategyAaveSupplyOnlyOptimism.sol (We took AAVE Optmism as an example)
    function beforeDeposit() external override {
        if (harvestOnDeposit) {
            require(msg.sender == vault, "!vault");
            _harvest(tx.origin);
        }
    }
```
4. [BIFI/strategies/Aave/StrategyAaveSupplyOnlyOptimism.sol#L124-L139](https://github.com/beefyfinance/beefy-contracts/blob/master/contracts/BIFI/strategies/Aave/StrategyAaveSupplyOnlyOptimism.sol#L124-L139)
```solidity
    // @audit Step4
                                         /* ︾ */  
    function _harvest( ... ) internal whenNotPaused gasThrottle {
        ...
    }
```

So if the Breedy strategy vault is paused, the depositing will revert, and can not occur.

## Recommended Mitigations
Checking if the vault is paused or not is not directly supported, we need to check the strategy contract itself. However since the beefy wrapper supports more than one Strategy, checking the state may differ from one Strategy to another, but if all Strategies have the same interface, the check can be implemented before depositing.

---
---
---

## [NC-01] Assets must precede the shares according to the ERC4626 standard

In the implementation of `PrizeVault::_burnAndWithdraw()`, the function takes shares then it takes the assets parameter at the end. And this is not the way assets and shares are provided to the ERC4626 functions.

[PrizeVault.sol#L887-L893](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L887-L893)
```solidity
    function _burnAndWithdraw(
        address _caller,
        address _receiver,
        address _owner,
        uint256 _shares, // @audit Order should be (_assets, _shares) as that of ERC4626
        uint256 _assets
    ) internal {
        ...
    }
```

All functions depositing/minting/withdrawing/redeeming in ERC4626 make the assets parameter first then shares.

## Recommended Mitigations
Make the assets first, then the shares at the last in `PrizeVault::_burnAndWithdraw()`.

_NOTE: You will have to change all the calling of this function in PrizeVault + testing scripts files_.


