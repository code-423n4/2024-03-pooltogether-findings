## [QA-01] No need to inherit `ERC20` by `TwabERC20` because `ERC20Permit` already inherits `ERC20`

## Relevant GitHub Links

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/TwabERC20.sol#L19

## Summary

OpenZeppelin's `ERC20Permit` inherits `ERC20` so there is no need to import and inherit `ERC20` in `TwabERC20`

## Recommended Mitigation Steps

Remove `ERC20` when declaring `TwabERC20`

```diff
    /// @dev    This contract is designed to be used as an accounting layer when building a vault
    ///         for PoolTogether V5.
    /// @dev    The TwabController limits all balances including total token supply to uint96 for
    ///         gas savings. Any mints that increase a balance past this limit will fail.
-   contract TwabERC20 is ERC20, ERC20Permit {
+   contract TwabERC20 is ERC20Permit {

    ////////////////////////////////////////////////////////////////////////////////
    // Public Variables
    ////////////////////////////////////////////////////////////////////////////////

    /// @notice Address of the TwabController used to keep track of balances.
    TwabController public immutable twabController;
```

## [QA-02] No need to use signature to approve assets if user has already approved more than enough assets in `PrizeVault.depositWithPermit()`

## Relevant GitHub Links

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L514-L546

## Summary

## Recommended Mitigation Steps

Call `permit()` only if user doesn't have enough approved tokens.

```diff
    // Skip the permit call if the allowance has already been set to exactly what is needed. This prevents
    // griefing attacks where the signature is used by another actor to complete the permit before this
    // function is executed.
-   if (_asset.allowance(_owner, address(this)) != _assets) {
+   if (_asset.allowance(_owner, address(this)) < _assets) {
        IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);
    }
```

## [QA-03] Use `safePermit()` instead of `permit` in `PrizeVault.depositWithPermit()`

## Relevant GitHub Links

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L540

## Summary

Some tokens can wrongly implement `ERC20Permit`. SafeERC20 library from openzeppelin have `safePermit()` function that provides stronger security that might help in some edge cases with tokens that wrongly implement `ERC20Permit`.

## Recommended Mitigation Steps

```diff
    // Skip the permit call if the allowance has already been set to exactly what is needed. This prevents
    // griefing attacks where the signature is used by another actor to complete the permit before this
    // function is executed.
    if (_asset.allowance(_owner, address(this)) != _assets) {
-       IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);
+       SafeERC20.safePermit(IERC20Permit(address(_asset)), _owner, address(this), _assets, _deadline, _v, _r, _s);
    }
```

## [QA-04] Frequently repeated `_maxWithdraw` calculation doesn't have separate function.

## Relevant GitHub Links

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L405

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L416

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L639

## Summary

3 functions in `PrizeVault` contract calculates `_maxWithdraw` by `_maxYieldVaultWithdraw() + _asset.balanceOf(address(this))`. But this operation does not have separate function even though it is used in few places.

## Recommended Mitigation Steps

Create function for `_maxYieldVaultWithdraw() + _asset.balanceOf(address(this))` operation and call it instead.