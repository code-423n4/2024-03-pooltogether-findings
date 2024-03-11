# QA Report: Pool Together

## L-01 yieldBuffer parameter cannot be adjusted

The `yieldBuffer` is designed to ensure that small, inevitable discrepancies between the assets actually held by the vault and the assets represented by the vault's shares (due to rounding errors in the underlying yield-generating mechanisms) do not lead to losses for depositors. This buffer acts as a safeguard, ensuring that there's always a small reserve of assets that can cover the difference, allowing depositors to withdraw their full entitlement even when the underlying investments might yield slightly less than expected.

Over time, several factors can change that might affect the adequacy of the initially set `yieldBuffer`

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L112

### Scenario

Adjusting `yieldBuffer` for Increased Yield Volatility

- Imagine you have deployed a PrizeVault with an initial `yieldBuffer` of 100 USDC, based on expected yield volatility and asset values at the time. This buffer was intended to cover rounding errors, ensuring depositors could always withdraw their full deposits.

- The vault operates smoothly, generating a predictable yield with minimal rounding errors. The 100 USDC buffer is more than sufficient to cover these discrepancies.

- Over time, the yield volatility increases significantly due to market fluctuations, and the underlying yield mechanism starts generating more significant rounding errors. Simultaneously, the value of the assets managed by the vault grows, increasing the absolute amount of funds affected by these rounding errors.

- If the `yieldBuffer` remains fixed at 100 USDC, it may no longer be sufficient to cover the rounding errors, risking the vault's ability to fulfill its "no loss" promise to depositors. In this scenario, depositors might start experiencing small losses, undermining the vault's integrity and depositor trust.

- Recognizing the change in yield volatility and asset value, the vault's governance decides to adjust the `yieldBuffer` to 200 USDC. This adjustment is made through a governance process, ensuring all stakeholders are aware and agree with the change.

- With the buffer adjusted, the vault is once again able to cover all rounding errors, ensuring depositors can withdraw their full entitlements. The trust in the vault's "no loss" promise is maintained, and operational efficiency is improved as the vault can continue to operate without risking depositor assets.

### Recommendation

Making the `yieldBuffer` adjustable allows the vault to adapt to changing market conditions and operational insights, maintaining its integrity and the trust of its depositors.

## L-02 Liquidation Pair Can Be Abused to Drain Contract Assets

If the contract owner has the unilateral authority to change the liquidation pair to a malicious or inefficient one, several issues could arise:

- A maliciously selected liquidation pair might offer poor exchange rates, causing the contract to receive less value for its yield than it should, effectively siphoning value away from the prize pool and depositors.
- If the liquidation pair is set up to interact with a malicious contract, it could lead to direct theft of assets from the `PrizeVault`.
- The owner could redirect funds to a controlled pair, withdrawing assets under the guise of liquidation but instead moving them to a personal account.

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L742

```solidity
function setLiquidationPair(address _liquidationPair) external onlyOwner {
    liquidationPair = _liquidationPair;
}
```

In this function, the `onlyOwner` modifier ensures only the contract owner can change the `liquidationPair`. This unchecked control is what enables abuse.

## L-03 Static Handling of Yield Contributions and Fees in PrizeVault

The PrizeVault contract is designed to generate yield from deposits and contribute this yield to a prize pool. However, the handling of yield contributions and fee percentages is static, not adapting to changes in yield rates, depositor engagement, or the prize pool's needs. This static approach can lead to "donation inflation," where the relative value or impact of the contributed yield diminishes over time.

The contract sets a fixed `yieldFeePercentage` and contributes yield to the prize pool without dynamic adjustments based on external conditions or the performance of the underlying assets.

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L119

```solidity
/// @notice Yield fee percentage represented in integer format with decimal precision defined by `FEE_PRECISION`.
/// @dev For example, if `FEE_PRECISION` were 1e9 a value of 1e7 = 0.01 = 1%.
uint32 public yieldFeePercentage;
```

And the function handling yield without considering dynamic conditions:

```solidity
function transferTokensOut(
    address,
    address _receiver,
    address _tokenOut,
    uint256 _amountOut
) public virtual onlyLiquidationPair returns (bytes memory) {

}
```

#### Impact:
- The static fee and yield contribution mechanism can result in a diminishing impact of prizes over time, particularly in changing market conditions.

- The static mechanism does not account for periods of high or low yield performance, potentially leading to either excessive or insufficient contributions to the prize pool.


#### Mitigation:
Implement a dynamic yield contribution and fee adjustment mechanism. This involves periodically reviewing and adjusting the `yieldFeePercentage` and the contribution strategy based on the vault's performance, current economic conditions, and the prize pool's needs.

Introduce a mechanism to adjust `yieldFeePercentage` dynamically:

```solidity
// Add a mechanism to adjust yield fees dynamically based on predefined criteria

function adjustYieldFeePercentage() external {
    uint32 newFeePercentage = calculateNewFeePercentage();
    require(newFeePercentage <= MAX_YIELD_FEE, "Exceeds maximum fee limit");
    yieldFeePercentage = newFeePercentage;
    emit YieldFeePercentageAdjusted(newFeePercentage);
}

function calculateNewFeePercentage() internal view returns (uint32) {
    // Implement logic to calculate new fee percentage based on the vault's performance and other factors
}
```

## L-04 Inaccurate Withdrawal Estimates Due to Unaccounted Slippage

The `PrizeVault` contract's `previewWithdraw` function provides users with an estimate of the shares needed to withdraw a specific amount of assets. However, it does not account for slippage that may occur during the actual withdrawal process. This oversight can lead to discrepancies between the estimated and actual amounts received by users, especially in volatile market conditions.

The `previewWithdraw` function calculates the number of shares needed for withdrawal based solely on the current asset balance and share price, without considering market conditions that could affect the actual withdrawal value. This approach fails to account for slippage in the underlying DeFi protocols or liquidity pools the `PrizeVault` might interact with.

```solidity
function previewWithdraw(uint256 _assets) public view returns (uint256) {
    uint256 _totalAssets = totalAssets();
    if (_totalAssets == 0) revert ZeroTotalAssets();
    uint256 totalDebt_ = totalDebt();
    if (_totalAssets >= totalDebt_) {
        return _assets;
    } else {
        return _assets.mulDiv(totalDebt_, _totalAssets, Math.Rounding.Up);
    }
}
```

This function returns the share equivalent of `_assets` without considering potential slippage, leading to inaccurate estimates.

### Scenario

Alice, a user of the `PrizeVault`, decides to withdraw a portion of her assets. She relies on the `previewWithdraw` function to understand how many assets she will receive for a certain number of shares. Given the volatility in the underlying market, the actual amount she receives upon withdrawal is significantly less than the estimate provided. This discrepancy impacts her financial planning, leading to dissatisfaction and mistrust towards the `PrizeVault`.

### Mitigation

Allow for dynamic adjustment of withdrawal estimates based on user-defined or recommended slippage tolerances.

## L-05 Incorrect Asset Distribution to users could lead to protocal insolvency

The core issue lies in the handling of the yield fee within the `transferTokensOut` function, where the fee is calculated but not subtracted from the amount being liquidated and sent to the receiver. This discrepancy means users receive more assets than they should after accounting for the intended yield fees, which contradicts the purpose of charging a fee on these transactions.

```solidity
// Increase yield fee balance:
if (_yieldFee > 0) {
    yieldFeeBalance += _yieldFee;
}

// Mint or withdraw amountOut to `_receiver`:
if (_tokenOut == address(_asset)) {
    _withdraw(_receiver, _amountOut);            
} else if (_tokenOut == address(this)) {
    _mint(_receiver, _amountOut);
}
```

In this code, `_yieldFee` is added to `yieldFeeBalance` without adjusting `_amountOut` that is actually sent to the `_receiver`. The result is an incorrect transaction where the fee's effect on the liquidation amount is not realized, leading to an over-distribution of assets to the user.


This behavior can lead to financial discrepancies within the `PrizeVault`:

- **For the Vault:** It results in less yield being collected as fees than intended, potentially affecting the vault's profitability or its ability to sustainably offer services and rewards.
- **For Users:** While initially appearing favorable since they receive more assets, this undermines the contractual and financial integrity of the vault, which can have long-term sustainability implications.

### Mitigation

Adjust the amount being liquidated (`_amountOut`) to account for the yield fee before executing the transfer or minting operation. 

```solidity
// Correctly applying the yield fee to the liquidation amount
if (_yieldFee > 0) {
    yieldFeeBalance += _yieldFee; // Record the fee
    _amountOut -= _yieldFee; // Adjust the liquidation amount by the fee
}

// Proceed to transfer the adjusted amount to the receiver
if (_tokenOut == address(_asset)) {
    _withdraw(_receiver, _amountOut);            
} else if (_tokenOut == address(this)) {
    _mint(_receiver, _amountOut);
}
```

### Scenario

- Emma, an active DeFi participant, uses `PrizeVault` for yield generation.
- Emma decides to liquidate a portion of her assets from `PrizeVault`.
- She anticipates a certain amount post-yield fee deduction based on the vault's fee policy.
- Emma receives more assets than expected; the yield fee isn't deducted from her withdrawal.
- The `transferTokensOut` function calculates but does not apply the yield fee to the withdrawal amount.
- Emma's initial surplus leads to concerns about the `PrizeVault`'s sustainability and fee policy integrity.
- While beneficial short-term, Emma worries about long-term implications for all vault participants.


