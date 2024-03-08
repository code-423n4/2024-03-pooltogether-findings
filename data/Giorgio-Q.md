## PrizeVault creation should revert if _tryGetAssetDecimals() fails instead of defaulting to 18 decimals

##Links

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L303-L305

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L772-L783

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L394-L399

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L319-L322

## Details

When a new PrizeVault is deployed through the factory, in order to asses the `_underlyingDecimals` variable, `_tryGetAssetDecimals()` will be fired. This call will return a bool and the assetDecimals. The problem is that if this call fails, the `_underlyingDecimals` variabe will default to 18 decimals. This is problematic because the shares are supposed to be initially on 1:1 ratio with the assets.

## Impact

If this `_tryGetAssetDecimals()` call in the constructor fails, assets and shares might have a different ratio if the 4626 asset has less or more than 18 decimals.

## Mitigation route

Instead of catching the failed call, revert if the bool returns false.


such as : 
```diff
        IERC20 asset_ = IERC20(yieldVault_.asset());
        (bool success, uint8 assetDecimals) = _tryGetAssetDecimals(asset_);
   ++   require(success, "unsucessful Call please try again");
        _underlyingDecimals = success ? assetDecimals : 18; 
``` 