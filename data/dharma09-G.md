## Pooltogether GAS OPTIMIZATION

## INTRODUCTION

Highlighted below are optimizations exclusively targeting state-mutating functions and view/pure functions invoked by state-mutating functions. In the discussion that follows, only runtime gas is emphasized, given its inevitable dominance over deployment gas costs throughout the protocol's lifetime.

Please be aware that some code snippets may be shortened to conserve space, and certain code snippets may include @audit tags in comments to facilitate issue explanations.

## Gas report

**Note: The issues addressed here were not reported by the bot, for packing variables, notes explaining the how and why are included**

## [G-01] State variables can be packed into fewer storage slot (saves 5 SLOTS: 10.2k Gas)
Note: The bot report did not cover these instances.

### Pack the these variable by reorder position (saves 2.1k Gas)

Here we can pack `FEE_PRECISION` and `MAX_YIELD_FEE` with yieldFeeRecipient.By doing this we can save one slot which helps to save 2.1k gas
### Proof of Code
- [PrizeVault.sol#L74C4-L80C48](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L74C4-L80C48)
```solidity
File: src/PrizeVault.sol
73: /// @notice The yield fee decimal precision.
 @>   uint32 public constant FEE_PRECISION = 1e9; //@audit
    /// @notice The max yield fee that can be set.
    /// @dev Decimal precision is defined by `FEE_PRECISION`.
    /// @dev If the yield fee is set too high, liquidations won't occur on a regular basis. If a use case requires
    /// a yield fee higher than this max, a custom liquidation pair can be set to manipulate the yield as required.
 @>   uint32 public constant MAX_YIELD_FEE = 9e8;
```
### Optimized code:

```diff
diff --git a/pt-v5-vault/src/PrizeVault.sol b/pt-v5-vault/src/PrizeVault.sol
index fafcff3..15a72e1 100644
--- a/pt-v5-vault/src/PrizeVault.sol
+++ b/pt-v5-vault/src/PrizeVault.sol
@@ -70,14 +70,6 @@ contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownab
     // Public Constants and Variables
     ////////////////////////////////////////////////////////////////////////////////
 
-    /// @notice The yield fee decimal precision.
-    uint32 public constant FEE_PRECISION = 1e9;
-    
-    /// @notice The max yield fee that can be set.
-    /// @dev Decimal precision is defined by `FEE_PRECISION`.
-    /// @dev If the yield fee is set too high, liquidations won't occur on a regular basis. If a use case requires
-    /// a yield fee higher than this max, a custom liquidation pair can be set to manipulate the yield as required.
-    uint32 public constant MAX_YIELD_FEE = 9e8;
 
     /// @notice The yield buffer that is reserved for covering rounding errors on withdrawals and deposits.
     /// @dev The buffer prevents the entire yield balance from being liquidated, which would leave the vault
@@ -117,7 +109,14 @@ contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownab
     /// @notice Yield fee percentage represented in integer format with decimal precision defined by `FEE_PRECISION`.
     /// @dev For example, if `FEE_PRECISION` were 1e9 a value of 1e7 = 0.01 = 1%.
     uint32 public yieldFeePercentage;
-
+    /// @notice The yield fee decimal precision.
+    uint32 public constant FEE_PRECISION = 1e9;
+    
+    /// @notice The max yield fee that can be set.
+    /// @dev Decimal precision is defined by `FEE_PRECISION`.
+    /// @dev If the yield fee is set too high, liquidations won't occur on a regular basis. If a use case requires
+    /// a yield fee higher than this max, a custom liquidation pair can be set to manipulate the yield as required.
+    uint32 public constant MAX_YIELD_FEE = 9e8;
     /// @notice Address of the yield fee recipient.
     address public yieldFeeRecipient;
```

## [G-02] `_totalDebt` stored in variables to avoid redundant calls within the function

### Details
 The totalSupply and _totalDebt are calculated only once and stored in variables to avoid redundant calls within the function.In `PrizeVault.sol`  calculate totaldebt depend on totalSupply which we can cache once then use it in our function. By doing this we can save gas.
### Proof of Code
- [PrizeVault.sol#L631C5-L655C1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L631C5-L655C1)
```solidity
File: src/PrizeVault.sol
630:  function liquidatableBalanceOf(address _tokenOut) public view returns (uint256) {
        uint256 _totalSupply = totalSupply();
        uint256 _maxAmountOut;
        if (_tokenOut == address(this)) {
            // Liquidation of vault shares is capped to the TWAB supply limit.
            _maxAmountOut = _twabSupplyLimit(_totalSupply);
        } else if (_tokenOut == address(_asset)) {
            // Liquidation of yield assets is capped at the max yield vault withdraw plus any latent balance.
            _maxAmountOut = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));
        } else {
            return 0;
        }

        // The liquid yield is computed by taking the available yield balance and multiplying it
        // by (1 - yieldFeePercentage), rounding down, to ensure that enough yield is left for the
        // yield fee.
        uint256 _liquidYield = 
@>            _availableYieldBalance(totalAssets(), _totalDebt(_totalSupply)) //@audit cache totalDebt uint256 totalDebt_ = _totalDebt(_totalSupply);
            .mulDiv(FEE_PRECISION - yieldFeePercentage, FEE_PRECISION);

        // The liquid yield is limited by the max that can be minted or withdrawn, depending on
        // `_tokenOut`.
        return _liquidYield >= _maxAmountOut ? _maxAmountOut : _liquidYield;
    }

```
### Optimized code:

```diff
diff --git a/pt-v5-vault/src/PrizeVault.sol b/pt-v5-vault/src/PrizeVault.sol
index fafcff3..1c5577a 100644
--- a/pt-v5-vault/src/PrizeVault.sol
+++ b/pt-v5-vault/src/PrizeVault.sol
@@ -630,6 +629,7 @@ contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownab
     /// @dev Supports the liquidation of either assets or prize vault shares.
     function liquidatableBalanceOf(address _tokenOut) public view returns (uint256) {
         uint256 _totalSupply = totalSupply();
+        uint256 totalDebt_ = _totalDebt(_totalSupply);
         uint256 _maxAmountOut;
         if (_tokenOut == address(this)) {
             // Liquidation of vault shares is capped to the TWAB supply limit.
@@ -645,7 +645,7 @@ contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownab
         // by (1 - yieldFeePercentage), rounding down, to ensure that enough yield is left for the
         // yield fee.
         uint256 _liquidYield = 
-            _availableYieldBalance(totalAssets(), _totalDebt(_totalSupply))
+            _availableYieldBalance(totalAssets(), totalDebt_)
             .mulDiv(FEE_PRECISION - yieldFeePercentage, FEE_PRECISION);
```
## [G-03] Avoid Redudant calculation through caching in local variable.

### Details
Pre-calculated Total Liquidation: The total liquidation amount (_amountOut + _yieldFee) is calculated outside the if statement to avoid redundant computations.

### Proof of Code
- [PrizeVault.sol#L672C7-L687C1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L672C7-L687C1)
```solidity
File: src/PrizeVault.sol
671:  if (_yieldFeePercentage != 0) {
            // The yield fee is calculated as a portion of the total yield being consumed, such that 
            // `total = amountOut + yieldFee` and `yieldFee / total = yieldFeePercentage`. 
            _yieldFee = (_amountOut * FEE_PRECISION) / (FEE_PRECISION - _yieldFeePercentage) - _amountOut;
        }

        // Ensure total liquidation amount does not exceed the available yield balance:
@>        if (_amountOut + _yieldFee > _availableYield) { //@audit cache 
            revert LiquidationExceedsAvailable(_amountOut + _yieldFee, _availableYield);
        }

        // Increase yield fee balance:
        if (_yieldFee > 0) {    
            yieldFeeBalance += _yieldFee;
        }
```
### Optimized code:

```diff
@@ -665,23 +665,22 @@ contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownab
         if (_amountOut == 0) revert LiquidationAmountOutZero();
 
         uint256 _availableYield = availableYieldBalance();

        uint32 _yieldFeePercentage = yieldFeePercentage;        
         // Determine the proportional yield fee based on the amount being liquidated:
         uint256 _yieldFee;
         if (_yieldFeePercentage != 0) {
             // The yield fee is calculated as a portion of the total yield being consumed, such that 
             // `total = amountOut + yieldFee` and `yieldFee / total = yieldFeePercentage`. 
            _yieldFee = (_amountOut * FEE_PRECISION) / (FEE_PRECISION - _yieldFeePercentage) - _amountOut;
         }

+        uint256 _totalliquidationAmount = _amountOut + _yieldFee;
         // Ensure total liquidation amount does not exceed the available yield balance:
-        if (_amountOut + _yieldFee > _availableYield) {
-            revert LiquidationExceedsAvailable(_amountOut + _yieldFee, _availableYield);
+        if (_totalliquidationAmount > _availableYield) { 
+            revert LiquidationExceedsAvailable(_totalliquidationAmount, _availableYield);
         }
 
         // Increase yield fee balance:
        if (_yieldFee > 0) {    
             yieldFeeBalance += _yieldFee;
         }
```
## [G-04] Use inline modifier if used only once through out whole contract
### Details
For better gas optimization is advisable to e inline require condition. that will check if caller either msg.sender or not
### Proof of Code
- [Claimable.sol#L52C2-L55C6](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L52C2-L55C6)
```solidity
File: src/abstract/Claimable.sol
52:   modifier onlyClaimer() { //@audit only use once
        if (msg.sender != claimer) revert CallerNotClaimer(msg.sender, claimer);
        _;
    }

```
## [G-05] Do not cache global variable in memory instead use global variable

### Details
- [PrizeVault.sol#L553](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L553)
```solidity
File:  src/PrizeVault.sol
552:  address _owner = msg.sender;
```