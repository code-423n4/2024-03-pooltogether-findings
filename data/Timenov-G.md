# PoolTogether Gas Optimizations by Timenov

## Summary

| |Issue|Instances|
|-|:-|:-:|
| G-01 | Default decimals can be returned. | 1 |
| G-02 | No need for parameter. | 1 |
| G-03 | Redundant operation. | 1 |

### [G-01] Default decimals can be returned.
Most of the ERC20 tokens have 18 decimals. Even if the `decimals()` function is not present, still the decimals will be 18. Thats why there is no need to return `(false, 0)` in `PrizeVault::_tryGetAssetDecimals` function and after that check if the return value is `false` and if so, set the decimals to `18`. Replace the return value of `0` to `18` and remove the `bool` return, as it will not be needed.

### [G-02] No need for parameter.
In `PrizeVault::depositWithPermit` there is no need for the `_owner` parameter, because after that we check if it is equal to the `msg.sender`. Consider removing the `_owner` parameter and use `msg.sender` instead.

### [G-03] Redundant operation.
In `PrizeVault::claimYieldFeeShares`, `_yieldFeeBalance` is set to the `yieldFeeBalance` state variable. After a validation, `yieldFeeBalance` is decreased by `_yieldFeeBalance`, which will be equal to `0` as the values did not change. Consider using `delete yieldFeeBalance;` instead.