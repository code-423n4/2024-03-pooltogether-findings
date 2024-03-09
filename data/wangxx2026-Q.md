## [L-01] The _maxYieldVaultWithdraw method can call yieldVault.maxWithdraw directly without having to compute the

ERC4626 defines the maxWithdraw method to get the maximum amount of assets the user can draw. It is recommended to call yieldVault.maxWithdraw directly instead of calling yieldVault.maxRedeem and then yieldVault.convertToAssets.

code:
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L921-L923

```solidity
    function _maxYieldVaultWithdraw() internal view returns (uint256) {
        return yieldVault.convertToAssets(yieldVault.maxRedeem(address(this)));
    }
```