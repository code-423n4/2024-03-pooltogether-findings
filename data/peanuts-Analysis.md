
### Summary and explanation of the protocol

- In poolTogether v5, users simply deposit into a prize vault to participate in the protocol
- The prize vault will generate yield from underlying yield contracts, and this yield will be liquidated and sent to the prize pool contract
- At given intervals, some users will win a prize, and this prize will be sent to the user
- If the user does not want to participate in the protocol anymore, simply withdraw the assets deposited into the protocol. 

- Note: The user will not earn any yield when depositing into the prize vault
- Note: The user is exposed to the risks of the underlying yield vault
- Note: Anyone can deploy a prize vault, but the connected underlying yield vault must be working and is currently generating yield

- Example: User A deposits 100 DAI into the PrizeVault that has an the sDAI vault as the underlying yield vault.
- Example: As yield accrues for sDAI, this yield is periodically withrawn and sent to the prize pool contract.
- Example: As the prize pool contract grows larger, users can win a bigger prize when they are chosen as the winner.
- Example: User A does not win anything. He withdraws his 100 DAI from the PrizeVault.

### Important Concepts

- Underlying yield vault refers to ERC4626 vaults that are generating yield, eg sDAI
- PrizeVault itself is an ERC4626 vault, but the assets are generally equal to the shares (unless negative yield/slippage/fees)
- The assets of the PrizeVault is the same as the assets of the ERC4626 underlying yield vault

### Codebase quality analysis

##### [PrizeVault.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol)

| Contract   | Function                | Access    | Comments                                                                                               |
| ---------- | ----------------------- | --------- | ------------------------------------------------------------------------------------------------------ |
| PrizeVault | constructor             | -         | sets yield fee recipient, percentage, yield vault, and prizepool. Note name and symbol refers to share |
| PrizeVault | decimals                | view      | returns decimals of asset, eg DAI                                                                      |
| PrizeVault | totalAssets             | view      | returns asset from shares conversion (eg sDAI -> DAI) and balance of DAI                               |
| PrizeVault | convertToShares         | -         | if underlying yield share devaluates, then asset worth more than share                                 |
| PrizeVault | convertToAssets         | -         | similar to above, rounding down is correct, should be 1:1                                              |
| PrizeVault | sponsor                 | -         | same as deposit, delegates to sponsorship address                                                      |
| PrizeVault | totalYieldBalance       | view      | total yield balance of the vault                                                                       |
| PrizeVault | currentYieldBuffer      | view      | gets the assets available in the current buffer                                                        |
| PrizeVault | claimYieldFeeShares     | YieldRecp | mints PrizeVault shares for the recipient and decrease yield fee balance                               |
| PrizeVault | setYieldFeePercentage   | onlyOwner | Set yield fee percentage, max of 9e8                                                                   |
| PrizeVault | liquidatableBalanceOf   | view      | Checks liquitable balance                                                                              |
| PrizeVault | transferTokensOut       | LiqPair   | Traansfer winnings out, not all winnings can be withdrawn, there's a buffer                            |
| PrizeVault | setClaimer              | onlyOwner | set the claimer, not part of this contract                                                             |
| PrizeVault | setLiquidationPair      | onlyOwner | Set liquidation pair, address zero checked                                                             |
| PrizeVault | setYieldFeePercentage   | onlyOwner | Set yield fee percentage, max of 9e8                                                                   |
| PrizeVault | setYieldFeeRecipient    | onlyOwner | Set yield fee recipient,                                                                               |
| PrizeVault | \_tryGetAssetDecimals   | internal  | Call ERC20 decimals to get the decimals back                                                           |
| PrizeVault | \_totalDebt             | internal  | Amount of share supply + yieldFeeBalance (that hasn't converted to shares yet)                         |
| PrizeVault | \_twabSupplyLimit       | internal  | Total supply of the PrizeVault share                                                                   |
| PrizeVault | \_totalYieldBalance     | internal  | How much yield PrizeVault earns                                                                        |
| PrizeVault | \_availableYieldBalance | internal  | How much yield the PrizeVault has earned minus a buffer                                                |
| PrizeVault | \_depositAndMint        | internal  | deposits assets, converts to yield shares, and get an equivalent amount of PrizeVault shares back      |
| PrizeVault | \_burnAndWithdraw       | internal  | burns the PrizeVault shares and calls \_withdraw                                                       |
| PrizeVault | \_maxYieldVaultWithdraw | internal  | How much assets to get back from how much underlying yield shares the PrizeVault has                   |
| PrizeVault | \_withdraw              | internal  | gets how much yield shares to burn for how much yield assets to get back                               |
| PrizeVault | \_setYieldFeePercentage | internal  | max bound of MAX_YIELD_FEE                                                                             |
| PrizeVault | \_setYieldFeeRecipient  | internal  | change yield fee recipient                                                                             |

**Notes**

- There are two vaults in question: Underlying vaults and the PrizeVault
- Each Vault has an asset an a share
- For the underlying vault, eg sDAI, the asset is DAI and the share is sDAI
- If sDAI is used as the underlying vault in the PrizeVault, then the asset of the PrizeVault is DAI and the shares of the PrizeVault is DAIshares (new)
- Usually, the asset and shares of the PrizeVault is 1:1 with each other. 

**Potential Issues** 
- In `transferTokensOut()`, yieldFee seems to be calculated wrongly
- In times of negative yield, the previewWithdraw function does not convert properly to assets
- YieldFee can accumulate, resulting in the yield fee recipient withdrawing the deposits of users

### Mechanism Review

**User Deposits**
- User simply deposit the asset (eg DAI) into the PrizeVault contract. They will be minted PrizeVault shares, which are 1:1 with the assets that they depositd
- The mechanism is easy to understand, users do not need to understand the complexity of the calculations. They can participate in the protocol easily
- The only check is probably whether the yield vault is actually generating yield, but that responsibility should be on the user.

**User Withdrawals**
- Similar to deposits, user simply withdraws their asset by burning their PrizeVault shares. Since shares are generally 1:1 with the asset, they will receive an equivalent amount of assets back
- This mechanism is also easy to understand and users do not need to know a lot to participate in the protocol

**Fee Collection**
- Fees are only taken when the yield is transferred out of the PrizeVault contract
- The fees is accounted internally
- When fees are collected, they are converted to PrizeVault shares
- There are some issues with this mechanism, as pointed out earlier. YieldFee is calculated incorrectly, and yield can be accumulated which puts the responsibility onto the fee recipient to withdraw amounts so as to not affect the withdrawal of users

**Creation of vaults**
- Creation of vaults cannot be frontrunned in case of a block reorg because of salt with msg.sender + nonce, so that's a good thing
- There is a small YIELD_BUFFER to be transferred to the vault, which is a good thing as well, to prevent rounding errors
- The yieldVault is not checked, the responsibility should lie on the user and the protocol to check the yield vault used
- PrizeVault creators control the yield fee recipient and percentage, and percentage can be set to 90% which is quite dangerous

### Centralization Risks

- Not much centralization risks, as the vaults are permissionlessly deployed. The risk would just be the individual vault deployers and the yield fee that they set, but it mostly affects the protocol and not the user

### Systemic Review

- For the collection of yield, it seems that it is a manual process, and so if gas fees are high, the yield may be offset because of the gas fees

### Time spent:
20 hours