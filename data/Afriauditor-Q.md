## [L-1]
Add zero address check to `_setYieldFeeRecipient`
```
function _setYieldFeeRecipient(address _yieldFeeRecipient) internal {
+         if (_yieldFeeRecipient == 0) revert
        yieldFeeRecipient = _yieldFeeRecipient;
        emit YieldFeeRecipientSet(_yieldFeeRecipient);
    }

```