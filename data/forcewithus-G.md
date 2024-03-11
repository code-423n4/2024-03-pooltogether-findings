Redundant parameters/functions

It is noted that the input of `targetOf` is not used and `prizePool` is immutable.
```solidity
    function targetOf(address) external view returns (address) {
        return address(prizePool);
    }
```

Consider removing the unused parameters/functions.