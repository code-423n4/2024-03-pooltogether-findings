## Impact

The YieldFeeRecipient can lose some yield shares. when YieldFeeRecipient call the claimYieldFeeShares(uint256 _shares) when  _shares < yieldFeeBalance , yieldFeeBalance should be decreased by shares YeildFeeBalance-= _shares but in the PrizeVault it is implemented by 
yieldFeeBalance -= _yieldFeeBalance; which results in 0 for yieldFeeBalance for every claimYieldFeeShares even if shares == YeildFeeBalance or not 

## Proof of Concept
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L617


## Recommended Mitigation Steps
The yieldFeeShares should be decreased by _shares