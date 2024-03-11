 ##  Avoid unnecessary function returns

## Description

* Optimize the contract by refraining from updating the function return when the value remains unchanged in this specific scenario ```(PrizeVault.sol#L-482 -> mint() -> previewMint()->)```. If the _shares value is identical to the new value, abstaining from re-storing it can prevent a ```SSTORE``` operation, which incurs a cost of ```2900 gas```. This strategy may involve a trade-off, potentially introducing a SLOAD operation (2100 gas) or a WARMACCESS operation (100 gas). Note that both ```previewDeposit()``` and ```previewMint()`` functions currently return the same value.


* https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L483

* https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L476


* There are 2 instances of this issue:

```
PrizeVault.sol -> 

function previewMint(uint256 _shares) public pure returns (uint256) {
        // shares represent how many assets an account has deposited, so they are 1:1 on mint
        return _shares;
    }


function previewDeposit(uint256 _assets) public pure returns (uint256) {
        // shares represent how many assets an account has deposited, so they are 1:1 on deposit
        return _assets;
    }

```