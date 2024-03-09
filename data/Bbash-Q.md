## Incorrect PPD calculation in comments           

### Relevant GitHub Links
	
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L101

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L46


## Summary
There are instances of wrong calculation of PPD ( Precision Per Dollar) in the comments.
## Vulnerability Details

Instances:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L101

    /// Precision per dollar (PPD) can be calculated by: (10 ^ DECIMALS) / ($ value of 1 asset).
    /// For example, USDC has a PPD of (10 ^ 6) / ($1) = 10e6 p/$.

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L46

    /// Precision per dollar (PPD) can be calculated by: (10 ^ DECIMALS) / ($ value of 1 asset).
    /// For example, USDC has a PPD of (10 ^ 6) / ($1) = 10e6 p/$.


According to the formula of PPD i.e., ``` (10 ^ DECIMALS) / ($ value of 1 asset)``` ,in both of the above instances, the PPD of USDC should be ```(10 ^ 6) / ($1) = 1e6 p/$```.

## Impact
Wrong calculation leads to confusion.
## Tools Used
Manual review 
## Recommendations
Update the PPD of USDC to ```1e6 p/$```.