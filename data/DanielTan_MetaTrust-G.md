# The `_depositAndMint` function repeatedly reads storage variables
The `_depositAndMint` function repeatedly reads the storage variable `_asset` 5 times, reads the storage variable `yieldVault` 4 times, which cost many gas due to the reading of storage variable is expensive.

Caching the state variables `_asset`, and `yieldVault` as following example will save many gas:


```
        ...
        IERC20 asset = _asset;
        asset.safeTransferFrom(
            _caller,
            address(this),
            _assets
        );
        ...
        IERC4626 _yieldVault = yieldVault;
        uint256 _yieldVaultShares = _yieldVault.previewDeposit(_assetsWithDust);
        ...
```

The `_depositAndMint` function is invoked by four functions `sponsor`, `depositWithPermit`, `mint`, and `deposit`. So, this gas optimazation will get serval times income.