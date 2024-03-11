## Impact
Slight incompliance with the ERC4626

## Proof of Concept

As per the overview of the contract: `"The new prize vault is designed to be fully compliant with the ERC4626 specification..."`. However, in the `PrizeVault` contract, two of the preview functions have got different `stateMutability` from the one stated in the [EIP4626](https://eips.ethereum.org/EIPS/eip-4626) - which are [previewDeposit](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L441), [previewMint](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L447). Although both of the functions don't change or read from any of the state variables, the state mutability should be updated to `view` to be compliant with the ERC-4626.

The `PrizeVault` contract inherits from OpenZeppelin's IERC4626, where both of the functions are with state mutability of view as well: [previewDeposit](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/interfaces/IERC4626.sol#L96),[previewMint](https://github.com/transmissions11/solmate/blob/main/src/tokens/ERC4626.sol#L140)

## Tools Used

Manual Review

## Recommended Mitigation Steps

Update the state mutability of both `previewDeposit` and `previewMint` to `view`:

```diff
-    function previewDeposit(uint256 _assets) public pure returns (uint256) {
+    function previewDeposit(uint256 _assets) public view returns (uint256) {
        // shares represent how many assets an account has deposited, so they are 1:1 on deposit
        return _assets;
    }

    /// @inheritdoc IERC4626
-    function previewMint(uint256 _shares) public pure returns (uint256) {
+    function previewMint(uint256 _shares) public view returns (uint256) {
        // shares represent how many assets an account has deposited, so they are 1:1 on mint
        return _shares;
    }
```