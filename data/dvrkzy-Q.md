# [L-01] PrizeVault assumes token has 18 decimals if _tryGetAssetDecimals() fails
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L305
## Proof of concept
Imagine the asset has 10 decimals and somehow the `_tryGetAssetDecimals()` returns `false` because it fails to retrieve the token decimals. Then the contract will assume that the asset has 18 decimals instead of 10.
## Mitigation
I recommend making the transaction fail if `_tryGetAssetDecimals()` returns false

# [L-02] PrizeVault will not work with tokens that revert on zero value approvals
## Summary
Some tokens (e.g. BNB) revert when approving a zero value amount (i.e. a call to `approve(address, 0))`.
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L869

# [L-03] Missing zero address check in `claimPrize()`
## Summary
If `_winner` is set to the zero address then the `_recipient` will also be set to the zero address.
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L76-L82

# [L-04] Missing `totalAssets == 0` check in `previewRedeem`
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L470-L472
You could argue that there is no point for checking if `totalAssets` is zero, because if it's zero then `previewRedeem()` will return 0 `_assets`. However this check is included in the `previewWithdraw()` function where if `totalAssets` is zero and the check is missing the transaction will simply revert because of devision by 0.
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L458
