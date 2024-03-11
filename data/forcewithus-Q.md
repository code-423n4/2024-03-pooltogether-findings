## Potential failure to call `_withdraw()`

It is noted that the contract uses the below functions to withdraw assets from the vault when `_assets > _latentAssets`.
```solidity
            // The latent balance is subtracted from the withdrawal so we don't withdraw more than we need.
            uint256 _yieldVaultShares = yieldVault.previewWithdraw(_assets - _latentAssets);
            // Assets are sent to this contract so any leftover dust can be redeposited later.
            yieldVault.redeem(_yieldVaultShares, address(this), address(this));
```

However, if all the shares are belong to one account(such as the first depositor or last holder), it may fail in the extreme edge case of the assets of the vault are less than the corresonding assets of the `_yieldVaultShares` since the `previewWithdraw()` uses `Math.Rounding.Ceil` to convert shares.