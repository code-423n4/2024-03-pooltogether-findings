unused parameter in function `maxMint` is being passed

`address _owner` is not being used in the `maxDeposit` even if is passed
this uses more gas and removing it will save gas in deployment


https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L397

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L374

```solidity
      function maxDeposit(address) public view returns (uint256) {
        uint256 _totalSupply = totalSupply();
        uint256 totalDebt_ = _totalDebt(_totalSupply);
        if (totalAssets() < totalDebt_) return 0;

        // the vault will never mint more than 1 share per asset, so no need to convert supply limit to assets
        uint256 twabSupplyLimit_ = _twabSupplyLimit(_totalSupply);
        uint256 _maxDeposit;
        uint256 _latentBalance = _asset.balanceOf(address(this));
        uint256 _maxYieldVaultDeposit = yieldVault.maxDeposit(address(this));
        if (_latentBalance >= _maxYieldVaultDeposit) {
            return 0;
        } else {
            unchecked {
                _maxDeposit = _maxYieldVaultDeposit - _latentBalance;
            }
            return twabSupplyLimit_ < _maxDeposit ? twabSupplyLimit_ : _maxDeposit;
        }
    }


   function maxMint(address _owner) public view returns (uint256) {
        return maxDeposit(_owner);
    }

```

