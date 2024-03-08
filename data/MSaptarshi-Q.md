# [L-01] The 0 address check for `receiver` is not done at the starting
The problem with this is any 0 address check should be done before any state change to that address 
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/abstract/Claimable.sol#L97
## Recommendation 
Shift the 0 address check at the starting of the function