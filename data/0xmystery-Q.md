## [L-01] PrizeVault._tryGetAssetDecimals() may return erroneous decimals
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

## [L-02] Rounding strategies in lossy states should prioritize protocol security
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
## [L-03] Streamlining token approvals in PrizeVault._depositAndMint() with forceApprove()
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
## [L-04] Simplifying yield fee distribution through direct transfers
Adopting a direct transfer approach via [_withdraw()](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L939) for handling `_yieldFee` payments to the `_yieldFeeRecipient`, as opposed to minting and claiming fee shares late via `claimYieldFeeShares()`, presents a streamlined method that enhances operational efficiency and transparency. This strategy effectively circumvents the complexities and potential issues associated with share-based fee claims, particularly in [nuanced scenarios such as lossy states](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L619) of the contract. Additionally, it eradicates the risk of losing the remainder if `yieldFeeBalance` [isn't fully claimed and minted](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L617). While direct transfers ensure immediate fee settlement and avoid the intricacies of share management, it's imperative to assess the implications on the contract's economic dynamics, potential increase in transaction costs, and the overarching security framework. A careful consideration and robust implementation of this approach can significantly contribute to the clarity and efficacy of fee handling mechanisms in decentralized finance applications, ensuring alignment with the contract's broader economic and operational goals.

## [L-05] Enhancing liquidation efficiency with dynamic adjustment
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

## [L-06] Adapting PrizeVault to L2's decentralized sequencing: Navigating New Frontiers in Transaction Fairness
As Layer 2 like Arbitrum or Base considers moving towards a more decentralized sequencer model, the platform faces the challenge of maintaining its current mitigation of frontrunning risks inherent in a "first come, first served" system. The transition could reintroduce vulnerabilities to transaction ordering manipulation, demanding innovative solutions to uphold transaction fairness. Strategies such as commit-reveal schemes, submarine sends, Fair Sequencing Services (FSS), decentralized MEV mitigation techniques, and the incorporation of time-locks and randomness could play pivotal roles. These measures aim to preserve the integrity of transaction sequencing, ensuring that the L2's evolution towards decentralization enhances its ecosystem without compromising the security and fairness that are crucial for user trust and platform reliability.

## [L-07] Enhancing contract efficiency with proactive financial health checks in PrizeVault._depositAndMint()
Incorporating early financial health checks within the PrizeVault contract's `_depositAndMint` function can significantly enhance operational efficiency and user experience by preemptively identifying a lossy state before executing any substantive contract actions. This proactive approach could avoid doomed transactions and reinforce the contract's integrity by ensuring that operations do not proceed under financially compromised conditions. While such early checks offer a clear benefit in terms of transaction efficiency and security, the dynamic nature of smart contract states necessitates a careful balance, possibly retaining end-of-operation validations to accommodate any changes occurring during transaction execution. This dual-check strategy ensures the PrizeVault remains responsive and reliable, even as it navigates the complexities of dynamic financial states within decentralized finance environments.

Here's the instance entailed that appears at the end of the call logic:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L874

```solidity
        if (totalAssets() < totalDebt()) revert LossyDeposit(totalAssets(), totalDebt()); 
```
## [L-08] Implementing dynamic adjustments for enhanced transaction reliability when depositing/withdrawing
Incorporating dynamic adjustments within the PrizeVault contract's [`deposit`, `mint`, `withdraw`, and `redeem` functions](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L474-L565) can significantly bolster transaction reliability and user experience by adapting to real-time changes in available assets or shares. Given the inherent variability of transaction execution times on the blockchain, which can lead to discrepancies between pre-checked limits (as ultimately determined by [`maxDeposit`, `maxMint`, `maxWithdraw`, and `maxRedeem`](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L368-L438)) and the contract's state at execution, integrating such adjustments ensures that transactions proceed smoothly without unnecessary reverts, even under fluctuating conditions. This approach not only enhances the contract's responsiveness to network dynamics but also aligns with user expectations for a seamless and efficient interaction with the contract, reinforcing trust and satisfaction within the platform.

## [N-01] Private function with embedded modifier reduces contract size
Consider having the logic of a modifier embedded through a private function to reduce contract size if need be. A `private` visibility that is more efficient on function calls than the `internal` visibility is adopted because the modifier will only be making this call inside the contract.

For instance, the modifier below may be refactored as follows:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L260-L265

```diff
+    function _onlyLiquidationPair() private view {
+        if (msg.sender != liquidationPair) {
+           revert CallerNotLP(msg.sender, liquidationPair);
+        }
+    }

     modifier onlyLiquidationPair() {
-        if (msg.sender != liquidationPair) {
-           revert CallerNotLP(msg.sender, liquidationPair);
-        }
+        _onlyLiquidationPair();
     _;
     }
```
## [N-02] Activate the Optimizer
Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number.

```
module.exports = {
solidity: {
version: "0.8.24",
settings: {
optimizer: {
  enabled: true,
  runs: 1000,
},
},
},
};
```
Please visit the following site for further information:

https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler

Here's one example of instance on opcode comparison that delineates the gas saving mechanism:

```
for !=0 before optimization
PUSH1 0x00
DUP2
EQ
ISZERO
PUSH1 [cont offset]
JUMPI

after optimization
DUP1
PUSH1 [revert offset]
JUMPI
```
Disclaimer: There have been several bugs with security implications related to optimizations. For this reason, Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past . A high-severity bug in the emscripten -generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. Please measure the gas savings from optimizations, and carefully weigh them against the possibility of an optimization-related bug. Also, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.