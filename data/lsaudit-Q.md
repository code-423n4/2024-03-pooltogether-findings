# [1] `convertToShares()` and `convertToAssets()` can be simplified

**File:** `PrizeVault.sol`

[File: pt-v5-vault/src/PrizeVault.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L341)
```solidity
341:     function convertToShares(uint256 _assets) public view returns (uint256) {
342:         uint256 totalDebt_ = totalDebt();
343:         uint256 _totalAssets = totalAssets();
344:         if (_totalAssets >= totalDebt_) {
345:             return _assets;
346:         } else {
347:             // If the vault controls less assets than what has been deposited a share will be worth a
348:             // proportional amount of the total assets. This can happen due to fees, slippage, or loss
349:             // of funds in the underlying yield vault.
350:             return _assets.mulDiv(totalDebt_, _totalAssets, Math.Rounding.Down);
351:         }
352:     }
```
The `else` statement at line 346 is redundant. When condition at line 344 won't be fulfulled, we will return with the value of `_assets.mulDiv(totalDebt_, _totalAssets, Math.Rounding.Down)`. The code can be simplified by removing the unnecessary `else` branch:

```
    function convertToShares(uint256 _assets) public view returns (uint256) {
        uint256 totalDebt_ = totalDebt();
        uint256 _totalAssets = totalAssets();
        if (_totalAssets >= totalDebt_) {
            return _assets;
        }
        // If the vault controls less assets than what has been deposited a share will be worth a
        // proportional amount of the total assets. This can happen due to fees, slippage, or loss
        // of funds in the underlying yield vault.
        return _assets.mulDiv(totalDebt_, _totalAssets, Math.Rounding.Down);          
    }
```

The same issue occurs in `convertToAssets()`:

[File: pt-v5-vault/src/PrizeVault.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L355)
```solidity
355:     function convertToAssets(uint256 _shares) public view returns (uint256) {
356:         uint256 totalDebt_ = totalDebt();
357:         uint256 _totalAssets = totalAssets();
358:         if (_totalAssets >= totalDebt_) {
359:             return _shares;
360:         } else {
361:             // If the vault controls less assets than what has been deposited a share will be worth a
362:             // proportional amount of the total assets. This can happen due to fees, slippage, or loss
363:             // of funds in the underlying yield vault.
364:             return _shares.mulDiv(_totalAssets, totalDebt_, Math.Rounding.Down);
365:         }
366:     }
```

When condition at line 358 won't be fulfilled, we can immediately return with `_shares.mulDiv(_totalAssets, totalDebt_, Math.Rounding.Down)`, instead of wrapping it in `else` block:

```
function convertToAssets(uint256 _shares) public view returns (uint256) {
        uint256 totalDebt_ = totalDebt();
        uint256 _totalAssets = totalAssets();
        if (_totalAssets >= totalDebt_) {
            return _shares;
        }
        // If the vault controls less assets than what has been deposited a share will be worth a
        // proportional amount of the total assets. This can happen due to fees, slippage, or loss
        // of funds in the underlying yield vault.
        return _shares.mulDiv(_totalAssets, totalDebt_, Math.Rounding.Down); 

    }
```

# [2] Setters should prevent re-setting of the same value

**File:** `PrizeVault.sol`

[File: pt-v5-vault/src/PrizeVault.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L753)
```solidity
753:     function setYieldFeePercentage(uint32 _yieldFeePercentage) external onlyOwner {
754:         _setYieldFeePercentage(_yieldFeePercentage);
755:     }
```

[File: pt-v5-vault/src/PrizeVault.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L947)
```solidity
947:     function _setYieldFeePercentage(uint32 _yieldFeePercentage) internal {
948:         if (_yieldFeePercentage > MAX_YIELD_FEE) {
949:             revert YieldFeePercentageExceedsMax(_yieldFeePercentage, MAX_YIELD_FEE);
950:         }
951:         yieldFeePercentage = _yieldFeePercentage;
952:         emit YieldFeePercentageSet(_yieldFeePercentage);
953:     }
```

It's possible to call function which updates the state variable with the same value.
It's a good practice to make sure, that whenever we update some value, it's not being set to the same value which it was before the update.
E.g., let's consider a scenario where `yieldFeePercentage = 7e7;`. Calling `setYieldFeePercentage(7e7)` is possible, even though it won't change the value of the variable: `yieldFeePercentage` will remain `7e7`.
Moreover, after calling `setYieldFeePercentage(123)`, function will still emit an `YieldFeePercentageSet()` event, even though nothing had been updated (the `yieldFeePercentage` would remain the same). Seeing that event might be confusing for the end-user.

Our recommendation is to implement additional check which verifies if value is really changed. This can be done in `_setYieldFeePercentage()` function, by adding additional `require` check:

```
require(yieldFeePercentage != _yieldFeePercentage, "Nothing to update!");
```

The same issue has been observed in the following instances:

[File: pt-v5-vault/src/PrizeVault.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L759)
```solidity
759:     function setYieldFeeRecipient(address _yieldFeeRecipient) external onlyOwner {
760:         _setYieldFeeRecipient(_yieldFeeRecipient);
761:     }
```

[File: pt-v5-vault/src/PrizeVault.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L958)
```solidity
958:     function _setYieldFeeRecipient(address _yieldFeeRecipient) internal {
959:         yieldFeeRecipient = _yieldFeeRecipient;
960:         emit YieldFeeRecipientSet(_yieldFeeRecipient);
961:     }
962: 
```

# [3] Typos

**File:** `PrizeVault.sol`

[File: pt-v5-vault/src/PrizeVault.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L40)
```solidity
40: ///                precision; if alice deposits 199 assets, the yield vault will round down on the conversion and mint
41: ///                alice 1 share, essentially donating the remaining 99 assets to the yield vault. This behavior can
```

`alice` should be changed to `Alice`.


# [4] Stick to one way of  `if`-conditions syntax

**File:** `PrizeVault.sol`

If conditional `if` contains single instruction, that instruction can be either inside curly brackets or not. E.g., these two syntax are valid:

```
if (condition) doSomething();
```

```
if (condition) {
    doSomething();
}
```

During the code-review process, we've detected that code-base uses both syntax. 

Using both syntax decreases the code readability. The example of mixed syntax can be noticed in the following instance:

[File: pt-v5-vault/src/PrizeVault.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L894)
```solidity
        if (_assets == 0) revert WithdrawZeroAssets();
        if (_shares == 0) revert BurnZeroShares();
        if (_caller != _owner) {
            _spendAllowance(_owner, _caller, _shares);
        }
```

As demonstrated above, lines 894, 895 use syntax without curly brackets, while lines 896-898 use curly brackets.

Our recommendation is to stick to one syntax and use it across the whole code-base. Our recommendation is to use syntax with curly brackets, since it increases the code readability. Below we present the list of lines with syntax without curly brackets.

[File: pt-v5-vault/src/PrizeVault.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L1)
```solidity
300:        if (address(yieldVault_) == address(0)) revert YieldVaultZeroAddress();
301:        if (owner_ == address(0)) revert OwnerZeroAddress();
[...]
377:    if (totalAssets() < totalDebt_) return 0;
[...]
458:    if (_totalAssets == 0) revert ZeroTotalAssets();
612:            if (_shares == 0) revert MintZeroShares();
[...]
615:        if (_shares > _yieldFeeBalance) revert SharesExceedsYieldFeeBalance(_shares, _yieldFeeBalance);
[...]
665:    if (_amountOut == 0) revert LiquidationAmountOutZero();
[...]
743:    if (address(_liquidationPair) == address(0)) revert LPZeroAddress();
[...]
844:        if (_shares == 0) revert MintZeroShares();
845:        if (_assets == 0) revert DepositZeroAssets();
[...]
874:    if (totalAssets() < totalDebt()) revert LossyDeposit(totalAssets(), totalDebt())
[...]
894:    if (_assets == 0) revert WithdrawZeroAssets();
895:    if (_shares == 0) revert BurnZeroShares();
```