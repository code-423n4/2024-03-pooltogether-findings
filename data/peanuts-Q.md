### [L-01] Percentage Fee for vault deployment can be set up to 90%

When deploying a vault, the deployer can set the yield percentage fee of up to 90%. Since this vault deployment is decentralized, the owner of the vault should have less control when collecting yield fee as the collection of fees will affect the total amount in the PrizePool which will ultimately affect the protocol (If there is little Prize in the prize pool, users are not incentivized to participate in the protocol).

Consider lowering the MAX_YIELD_FEE to about 10% (1e18) so that the deployer will not get most of the yield.

```
    uint32 public constant MAX_YIELD_FEE = 9e8;

        function _setYieldFeePercentage(uint32 _yieldFeePercentage) internal {
        if (_yieldFeePercentage > MAX_YIELD_FEE) {
            revert YieldFeePercentageExceedsMax(_yieldFeePercentage, MAX_YIELD_FEE);
        }
        yieldFeePercentage = _yieldFeePercentage;
        emit YieldFeePercentageSet(_yieldFeePercentage);
    }
```

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L947-L953

### [L-02] Protocol is not ERC4626 compatible 

previewDeposit and previewMint simply returns the input amount, which should not be the case in the event of negative yield

```
    /// @inheritdoc IERC4626
    function previewDeposit(uint256 _assets) public pure returns (uint256) {
        // shares represent how many assets an account has deposited, so they are 1:1 on deposit
        return _assets;
    }


    /// @inheritdoc IERC4626
    function previewMint(uint256 _shares) public pure returns (uint256) {
        // shares represent how many assets an account has deposited, so they are 1:1 on mint
        return _shares;
    }
```

Also, the ERC4626 preview functions does convertToAsset and convertToShares.

```
    /** @dev See {IERC4626-previewMint}. */
    function previewMint(uint256 shares) public view virtual returns (uint256) {
        return _convertToAssets(shares, Math.Rounding.Ceil);
    }


    /** @dev See {IERC4626-previewWithdraw}. */
    function previewWithdraw(uint256 assets) public view virtual returns (uint256) {
        return _convertToShares(assets, Math.Rounding.Ceil);
    }
```