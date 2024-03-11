### [G-1] The `LiquidationPair` state should switched with above state to save a Storage slot

**Description** As we see in this states layout with their corresponding bytes:
```java
    uint32 public yieldFeePercentage; // 4 bytes

    address public yieldFeeRecipient; // 12 bytes

    uint256 public yieldFeeBalance; // 32 bytes

    // @audit-Gas this should be switched with `yieldFeeBalance` to save gas
    address public liquidationPair; // 12 bytes
```
if we switch the place of the `liquidationPair` and `yieldFeeBalance` then there will be one less storage slot which leads to saving gas

**Recommend Mitigation** switch the place of the `liquidationPair` and `yieldFeeBalance` states
```diff
	uint32 public yieldFeePercentage; // 4 bytes

    address public yieldFeeRecipient; // 12 bytes

+   address public liquidationPair; // 12 bytes
-   uint256 public yieldFeeBalance; // 32 bytes

+    uint256 public yieldFeeBalance; // 32 bytes
-    address public liquidationPair; // 12 bytes
```

