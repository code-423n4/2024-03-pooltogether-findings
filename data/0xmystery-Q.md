## PrizeVault._tryGetAssetDecimals() may return erroneous decimals
Some ERC20 assets do not have `decimals()` implemented. As such, when calling `PrizeVault._tryGetAssetDecimals()`, `success == false` when returned by `staticcall()`. And, `_tryGetAssetDecimals()` returns (false, 0):  

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L772-L783

```solidity
    function _tryGetAssetDecimals(IERC20 asset_) internal view returns (bool, uint8) {
        (bool success, bytes memory encodedDecimals) = address(asset_).staticcall(
            abi.encodeWithSelector(IERC20Metadata.decimals.selector)
        );
        if (success && encodedDecimals.length >= 32) {
            uint256 returnedDecimals = abi.decode(encodedDecimals, (uint256));
            if (returnedDecimals <= type(uint8).max) {
                return (true, uint8(returnedDecimals));
            }
        }
        return (false, 0);
    }
```
According to the logic implemented in the constructor, `_underlyingDecimals` would default to 18:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L303-L305

```solidity
        IERC20 asset_ = IERC20(yieldVault_.asset());
        (bool success, uint8 assetDecimals) = _tryGetAssetDecimals(asset_);
        _underlyingDecimals = success ? assetDecimals : 18;
```
This can lead to significant discrepancy if the non-standard asset decimals is different than 18. It can affect various contract functionalities, such as asset calculations and distributions, especially for tokens with non-standard decimal values when accessing `PrizeVault.decimals()`:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L320-L322

```solidity
    function decimals() public view override(ERC20, IERC20Metadata) returns (uint8) {
        return _underlyingDecimals;
    }
```
To mitigate this, contracts should be designed with mechanisms to accurately determine and use the correct decimal value, either by requiring decimal specification upon initialization, implementing fallback mechanisms with predefined mappings, or including validation checks to ensure compatibility and accuracy in token-related operations. Addressing this challenge is crucial for maintaining the reliability and fairness of smart contract transactions involving a diverse range of ERC20 tokens. 

## Rounding strategies in lossy states should prioritize protocol security
In scenarios where the prize vault enters a lossy state, adopting a conservative rounding strategy that favors the protocol, typically rounding down during conversions, becomes crucial for safeguarding its overall health and longevity. This approach ensures that the protocol does not exacerbate its lossy condition by over-issuing assets, thereby preserving its reserves and maintaining a buffer against further losses. While this might result in slightly less favorable outcomes for users in the short term, it is a necessary trade-off to ensure the protocol's stability and secure the interests of all participants in the long run. Prioritizing protocol security through conservative rounding in lossy states is a fundamental risk management strategy that helps in sustaining user trust and protocol viability during challenging times.

Consider having the following function refactored so that anyone calling it will not be given a faulty information vs the rounding up logic that has been correctly implemented in [PrizeVault.previewWithdraw()](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L465):

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L355-L366

```diff
    function convertToAssets(uint256 _shares) public view returns (uint256) {
        uint256 totalDebt_ = totalDebt();
        uint256 _totalAssets = totalAssets();
        if (_totalAssets >= totalDebt_) {
            return _shares;
        } else {
            // If the vault controls less assets than what has been deposited a share will be worth a
            // proportional amount of the total assets. This can happen due to fees, slippage, or loss
            // of funds in the underlying yield vault.
-            return _shares.mulDiv(_totalAssets, totalDebt_, Math.Rounding.Down);
+            return _shares.mulDiv(_totalAssets, totalDebt_, Math.Rounding.Up);
        }
    }
```
## Streamlining token approvals in PrizeVault._depositAndMint() with forceApprove()
Incorporating `forceApprove()` such as in `PrizeVault._depositAndMint()` could offer a more streamlined approach to managing ERC20 token allowances, particularly in complex interactions involving asset transfers to yield vaults. By enabling the contract to set precise allowances in a single step, this method could eliminate the need for conditional checks and subsequent resetting of allowances, thus simplifying the logic and potentially enhancing contract efficiency. 

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L860-L872

```diff
        // Previously accumulated dust is swept into the yield vault along with the deposit.
        uint256 _assetsWithDust = _asset.balanceOf(address(this));
-        _asset.approve(address(yieldVault), _assetsWithDust);
+        _asset.forceApprove(address(yieldVault), _assetsWithDust);

        // The shares are calculated and then minted directly to mitigate rounding error loss.
        uint256 _yieldVaultShares = yieldVault.previewDeposit(_assetsWithDust);
        uint256 _assetsUsed = yieldVault.mint(_yieldVaultShares, address(this));
-        if (_assetsUsed != _assetsWithDust) {
-            // If some latent balance remains, the approval is set back to zero for weird tokens like USDT.
-            _asset.approve(address(yieldVault), 0);
-        }

        _mint(_receiver, _shares);
```
## Simplifying yield fee distribution through direct transfers
Adopting a direct transfer approach via [_withdraw()](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L939) for handling `_yieldFee` payments to the `_yieldFeeRecipient`, as opposed to minting and claiming fee shares late via `claimYieldFeeShares()`, presents a streamlined method that enhances operational efficiency and transparency. This strategy effectively circumvents the complexities and potential issues associated with share-based fee claims, particularly in [nuanced scenarios such as lossy states](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L619) of the contract. Additionally, it eradicates the risk of losing the remainder if `yieldFeeBalance` [isn't fully claimed and minted](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L617). While direct transfers ensure immediate fee settlement and avoid the intricacies of share management, it's imperative to assess the implications on the contract's economic dynamics, potential increase in transaction costs, and the overarching security framework. A careful consideration and robust implementation of this approach can significantly contribute to the clarity and efficacy of fee handling mechanisms in decentralized finance applications, ensuring alignment with the contract's broader economic and operational goals.

## Enhancing liquidation efficiency with dynamic adjustment
Integrating a dynamic adjustment feature within the `transferTokensOut` function of PrizeVault.sol offers a robust solution to gracefully handle concurrent liquidations, significantly reducing the risk of transaction reverts:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L678-L681

```solidity
        // Ensure total liquidation amount does not exceed the available yield balance:
        if (_amountOut + _yieldFee > _availableYield) {
            revert LiquidationExceedsAvailable(_amountOut + _yieldFee, _availableYield);
        }
```
due to fluctuating available yield balances:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L653

```solidity
        return _liquidYield >= _maxAmountOut ? _maxAmountOut : _liquidYield;
```
This approach ensures that liquidation amounts are adjusted in real-time based on the latest `liquidatableBalanceOf`, thereby maintaining operational continuity and fairness among transactions, even in the face of potential frontrunning or high transaction volumes. 
