## Summary
In PrizeVault contract, the following error is not being used anywhere: 
```solidity
/// @notice Thrown when `sweep` is called but no underlying assets are currently held by the Vault.
    error SweepZeroAssets(); 
```
This can be commented out and deployment cost can be reduced. 

## POC

Deployment cost without commenting the unused error declaration : 
| src/PrizeVault.sol:PrizeVault contract |                 |        |        |        |         |
|----------------------------------------|-----------------|--------|--------|--------|---------|
| Deployment Cost                        | Deployment Size |        |        |        |         |
| 3997851                                | 20906           |        |

Deployment cost after : 
src/PrizeVault.sol:PrizeVault contract |                 |        |        |        |         |
|----------------------------------------|-----------------|--------|--------|--------|---------|
| Deployment Cost                        | Deployment Size |        |        |        |         |
| 3977981                                | 20906       

Thus, we can save ` 3997851 - 3977981 = 19870 ` gas.
