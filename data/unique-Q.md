## \[L-01\] Stop Using Solidity's transfer() Now

**Proof Of Concept**  
https://consensys.io/diligence/blog/2019/09/stop-using-soliditys-transfer-now/

```
101:    twabController.transfer(_from, _to, SafeCast.toUint96(_amount));
```

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/TwabERC20.sol#L101

&nbsp;

## \[N-02\] Showing the actual values of numbers in NatSpec comments makes checking and reading code easier.

```diff
File: src/PrizeVault.sol

- 74:	uint32 public constant FEE_PRECISION = 1e9;
+ 74:   uint32 public constant FEE_PRECISION = 1e9; // 1,000,000,000

- 80:     uint32 public constant MAX_YIELD_FEE = 9e8;
+ 80:     uint32 public constant MAX_YIELD_FEE = 9e8; // 900_000_000
```

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L74

```diff
- 63:   uint256 public constant YIELD_BUFFER = 1e5;

+ 63:   uint256 public constant YIELD_BUFFER = 1e5; // 100_000
```

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVaultFactory.sol#L63

## \[N-03\] For immutable variables in Solidity, the convention is to use CAPS\_CASE or UPPER\_CASE\_WITH\_UNDERSCORES.

```
File: src/PrizeVault.sol

112: uint256 public immutable yieldBuffer;

115: IERC4626 public immutable yieldVault;

135: IERC20 private immutable _asset;

138: uint8 private immutable _underlyingDecimals;
```

&nbsp;https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L112

```
26: TwabController public immutable twabController;
```

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/TwabERC20.sol#L26

&nbsp;

## \[N-04\] For multiple lines use NetSpec Comment for better readability

```
-	 /// @title  PoolTogether V5 Prize Vault
-        /// @author G9 Software Inc.
-	/// @notice The prize vault takes deposits of an asset and earns yield ...

+	/**
+    *  @title PoolTogether V5 Prize Vault
+    *  @author G9 Software Inc.
+    *  @notice The prize vault takes deposits of an asset and earns yield with ...
+    * 
+    */
```

> All contest

&nbsp;

## \[N-05\] Function writing that does not comply with the Solidity Style Guide

Context  
All Contracts

Description  
Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered:

constructor  
receive function (if exists)  
fallback function (if exists)  
external  
public  
internal  
private  
within a grouping, place the view and pure functions last

## \[S-01\] You can explain the operation of critical functions in NatSpec with an infographic.

&nbsp;

##   Conclusion

The code is well structured and organised into separate contracts, each with a clear purpose. NatSpec comments are well used throughout the codebase. The automated findings show that some styling conventions which improve the readability of the code.

&nbsp;