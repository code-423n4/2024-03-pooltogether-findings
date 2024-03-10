# [NC/G-1] - PrizeVault::totalYieldBalance can be removed
- `totalYieldBalance` is not used in the contract instead `_totalYieldBalance` is used directly
```
    //@audit-info function not used anywhere in the contract
    //_totalYieldBalance is used everywhere directly
    function totalYieldBalance() public view returns (uint256) {
        return _totalYieldBalance(totalAssets(), totalDebt());
    }
```