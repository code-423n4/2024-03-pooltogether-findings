### Advanced Analysis Report for [PoolTogetherV5](https://github.com/code-423n4/2024-03-pooltogether) by K42

#### Overview

The [PoolTogetherV5](https://github.com/code-423n4/2024-03-pooltogether) prize vault contracts are designed to enable a "no loss" prize savings system, where users can deposit assets, earn yield, and be eligible for prizes without risking their principal. The core contracts in scope are `PrizeVault`, `PrizeVaultFactory`, `TwabERC20`, `Claimable`, and `HookManager`.

#### Understanding the Ecosystem:

The prize vault ecosystem revolves around several key interactions:

1. Users deposit assets into a prize vault, which are then deposited into an underlying yield vault (e.g., ``Aave``, ``Compound``) to generate yield.
2. The generated yield is periodically liquidated by a designated liquidation pair contract and contributed to a central prize pool as prize tokens.
3. When a user wins a prize, a designated claimer contract can claim the prize on their behalf, with optional hooks for custom actions before and after the claim.
4. User balances are stored in a ``TwabController`` contract, which maintains time-weighted average balances for each user, enabling fair prize distribution.

#### Codebase Quality Analysis:

**1. Key Data Structures and Libraries**

- The contracts utilize the ``OpenZeppelin`` library for standard ``ERC20`` functionality, safe math operations, and access control mechanisms. This is a well-audited and widely used library, reducing the risk of common vulnerabilities.
- Custom data structures, such as `VaultHooks`, are clearly defined and used appropriately throughout the codebase, enhancing code readability and maintainability.

**2. Use of Modifiers and Access Control**

- The contracts make extensive use of modifiers for access control, such as `onlyLiquidationPair`, `onlyYieldFeeRecipient`, `onlyClaimer`, and `onlyOwner`. These modifiers ensure that only authorized entities can perform specific actions, reducing the risk of unauthorized access.
- The modifiers are applied consistently throughout the codebase, demonstrating a strong adherence to the principle of least privilege.

**3. Use of State Variables**

- State variables are clearly defined and well-documented, including important constants like `FEE_PRECISION` and `HOOK_GAS`. This improves code readability and helps in understanding the purpose and usage of each variable.
- Critical state variables, such as `yieldVault`, `yieldFeeRecipient`, and `claimer`, are properly initialized in the constructor and have associated setter functions with appropriate access control. This ensures that these variables can only be modified by authorized entities.

**4. Use of Events and Logging**

- The contracts emit events for all significant state changes and actions, providing transparency and facilitating off-chain monitoring and indexing. This is crucial for auditing and tracking the behaviour of the system.
- Key events, such as `Sponsor`, `TransferYieldOut`, `ClaimYieldFeeShares`, and `SetHooks`, cover crucial interactions within the prize vault ecosystem, enabling thorough monitoring and analysis.

**5. Key Functions that need special attention**

- `transferTokensOut`: This function is called by the liquidation pair to transfer yield tokens out of the prize vault. It is critical to ensure proper access control and mathematical correctness to prevent unauthorized withdrawals. The function includes checks to validate the caller and the liquidation amount, mitigating potential risks.
- `claimPrize`: This function is used by the claimer contract to award prizes to winners. The interaction with prize claim hooks and the prize pool contract is complex and requires more thorough testing. The function includes checks to validate the prize parameters and executes the claim process with the prize pool contract, ensuring the correct handling of prizes.
- `_depositAndMint` / `_burnAndWithdraw`: These internal functions handle the core deposit and withdrawal logic, interacting with the underlying yield vault and updating internal accounting. The correctness of the math and the proper sequencing of operations are critical. The functions include checks to validate the deposit and withdrawal amounts, handle rounding errors, and update the internal balances and shares accurately.

**6. Upgradability**

- The contracts do not implement any explicit upgradability mechanisms, such as proxy patterns or delegatecalls to mutable logic contracts. This reduces the complexity and potential attack surface associated with upgradable contracts.
- However, certain key components, like the claimer, liquidation pair, and yield fee recipient, can be updated by the contract owner. This provides flexibility for future changes or improvements without requiring a full contract upgrade.

#### Architecture Recommendations:

- Consider implementing a pause/emergency stop mechanism to halt critical operations (e.g., deposits, withdrawals, prize claims) in case of a detected vulnerability or attack. This would allow the contract owner to quickly respond to potential threats and mitigate risks.
- Explore ways to introduce upgradability for the core prize vault logic, allowing for bug fixes and improvements while ensuring the security of user funds. This could be achieved through the use of a transparent proxy pattern or a plugin-based architecture.

#### Centralization Risks:

- The contract owner has significant control over the prize vault, with the ability to update critical parameters such as the claimer, liquidation pair, yield fee percentage, and yield fee recipient. While this provides flexibility, it also introduces centralization risks.
- To mitigate these risks, consider implementing a multi-signature mechanism for critical owner functions, requiring multiple trusted parties to approve changes. Additionally, establish clear governance processes and transparency around any changes made by the owner.

#### Mechanism Review:

The core mechanisms of the prize vault system look to be well-designed and thought out:

1. User deposits are accurately tracked using the ``TwabController`` contract, ensuring fair distribution of prizes based on time-weighted average balances.
2. The generated yield is periodically liquidated by the designated liquidation pair contract, converting it into prize tokens. The liquidation process includes checks to validate the prize token and the liquidation amount, ensuring the correct handling of yield.
3. Prize claims are handled by the authorized claimer contract, with the flexibility for custom integrations through the use of before and after claim hooks. The claim process interacts with the prize pool contract to award prizes to winners accurately.

However, there are some potential issues to consider:

- The flexibility of the hook mechanism allows users to specify custom contracts to be called during the prize claim process. While this enables powerful customization, it also introduces potential risks if the hook contracts contain vulnerabilities or malicious code. Implementing strict validation and security checks for user-specified hook contracts is essential to mitigate these risks.

#### Systemic Risks:

- The prize vault's dependence on external contracts, such as the underlying yield vault and the prize pool contract, introduces systemic risks. A failure or security breach in any of these components could have cascading effects on the prize vault ecosystem.
- To mitigate these risks, it is crucial to thoroughly monitor the external dependencies, establish fallback mechanisms, and maintain clear communication channels with the relevant stakeholders.

#### Areas of Concern:

1. **Mathematical Correctness**: The prize vault contracts involve complex calculations related to yield accrual, fee deductions, and share conversions. While the contracts include checks to handle edge cases and rounding errors, it is essential to thoroughly test and validate the mathematical correctness of these calculations to prevent any potential exploits or inaccuracies.

2. **Access Control**: The contracts rely heavily on access control mechanisms to restrict critical operations to authorized entities. While the current implementation follows best practices, it is crucial to regularly review the access control logic to identify any potential vulnerabilities or loopholes.

3. **External Dependencies**: The prize vault contracts integrate with external systems, such as the underlying yield vault and the prize pool contract. Monitoring these external dependencies is crucial to ensure the overall security and reliability of the prize vault ecosystem.

4. **User-Specified Hook Contracts**: The flexibility for users to specify custom hook contracts during the prize claim process introduces potential risks. Implementing strict validation and security checks for user-specified hook contracts is essential to mitigate the risks of malicious or vulnerable contracts disrupting the system.

#### In Depth Codebase Analysis:

## `PrizeVault` Contract

### Functions and Risks in `PrizeVault`

#### `constructor`

- **Specific Risk**: The constructor function sets critical parameters such as the underlying yield vault, prize pool, claimer, yield fee recipient, and owner. If any of these parameters are incorrectly set or pointing to malicious contracts, it could compromise the security and integrity of the prize vault from the start.

- **Recommendation**: Ensure that all constructor parameters are thoroughly validated and point to trusted contracts. Implement additional checks to verify the correctness and security of these parameters, such as checking for non-zero addresses and confirming the expected contract interfaces.

#### `depositWithPermit`

- **Specific Risk**: The `depositWithPermit` function allows users to deposit assets into the prize vault using a signed permit, eliminating the need for a separate approval transaction. However, if the signature verification or the handling of the permit is flawed, it could allow unauthorized deposits on behalf of users.

- **Recommendation**: Thoroughly review and test the implementation of the permit mechanism, focusing on the signature verification process and the handling of the permit data. Ensure that the `ecrecover` function is used correctly and that the recovered address matches the expected depositor. Consider validating additional permit parameters, such as the expiration timestamp and the approved amount.

#### `sponsor`

- **Specific Risk**: The `sponsor` function allows anyone to deposit assets on behalf of another user and automatically delegate their tokens to the sponsorship address. While this feature can be useful for onboarding new users, it also introduces the risk of users being subjected to unexpected tax liabilities or having their prize tier affected without their explicit consent.

- **Recommendation**: Consider implementing an "opt-in" mechanism for sponsorship, where users must explicitly enable the ability for others to sponsor deposits on their behalf. This can be achieved through an additional mapping or a flag that users can set using a separate function. Additionally, consider emitting events to notify users when their tokens are sponsored and delegated.

#### `transferTokensOut`

- **Specific Risk**: The `transferTokensOut` function is called by the liquidation pair contract to transfer yield tokens out of the prize vault. If the access control mechanism (`onlyLiquidationPair` modifier) is flawed or if the liquidation pair address is compromised, it could lead to unauthorized withdrawals and loss of funds.

- **Recommendation**: Ensure that the `onlyLiquidationPair` modifier is implemented correctly and that only the designated liquidation pair contract can call this function. Regularly monitor the liquidation pair address for any suspicious activities or changes. Consider implementing additional checks, such as verifying the recipient address and the transferred token amount, to prevent unintended transfers.

#### `_depositAndMint` / `_burnAndWithdraw`

- **Specific Risk**: These internal functions handle the core deposit and withdrawal logic, interacting with the underlying yield vault and updating the internal accounting. Any errors or vulnerabilities in these functions could lead to incorrect minting or burning of shares, loss of funds, or exploitation by malicious actors.

- **Recommendation**: Thoroughly test these functions to ensure the correctness of the math operations, the proper handling of edge cases, and the correct sequencing of operations. Pay special attention to the interactions with the underlying yield vault, such as the share calculations and the handling of vault-specific errors.

## `PrizeVaultFactory` Contract

### Functions and Risks

#### `deployVault`

- **Specific Risk**: The `deployVault` function allows anyone to deploy a new prize vault with arbitrary parameters, including the underlying yield vault, prize pool, claimer, yield fee recipient, and owner. If the factory contract is not properly secured or if the deployment parameters are not validated, it could lead to the creation of malicious or poorly configured prize vaults.

- **Recommendation**: Consider adding a whitelist of approved yield vaults, prize pools, and other critical components to prevent the deployment of malicious prize vaults. Additionally, consider implementing access control mechanisms to restrict the deployment of new prize vaults to trusted entities or requiring multi-party approval for vault creation.

## `TwabERC20` Contract

### Functions and Risks

#### `_transfer`, `_mint`, `_burn`

- **Specific Risk**: These internal functions directly interact with the TwabController contract to update user balances and checkpoint the time-weighted average balances. If there are any vulnerabilities or errors in the TwabController contract, it could lead to incorrect balance updates, loss of funds, or potential exploitation.

- **Recommendation**: Test the ``TwabController`` contract to ensure its correctness and security. Pay special attention to the balance update and checkpointing mechanisms, ensuring that they are implemented correctly and resistant to manipulation or exploitation. Consider adding additional validation checks in the `TwabERC20` functions to verify the correctness of the ``TwabController`` responses and handle potential errors gracefully.

## `Claimable` Contract

### Functions and Risks

#### `claimPrize`

- **Specific Risk**: The `claimPrize` function allows the designated claimer contract to claim prizes on behalf of winners, with the option to execute user-specified hook contracts before and after the claim process. If the hook contracts contain vulnerabilities or malicious code, they could potentially exploit the prize vault or disrupt the prize claiming process.

- **Recommendation**: Implement a robust validation mechanism for user-specified hook contracts, such as a whitelist of approved contracts or a set of security checks to ensure that the hook contracts adhere to a specific interface and do not contain any known vulnerabilities or malicious code.

## `HookManager` Contract

### Functions and Risks

#### `setHooks`

- **Specific Risk**: The `setHooks` function allows users to specify arbitrary hook contracts that will be executed during the prize claiming process. If a user sets a malicious or vulnerable hook contract, it could potentially exploit the prize vault, steal funds, or disrupt the claiming process for other users.

- **Recommendation**: Implement a whitelist of approved hook contracts or a set of security checks to ensure that the specified hook contracts adhere to a specific interface and do not contain any known vulnerabilities or malicious code. Consider implementing additional safeguards, such as gas limits and reentrancy guards, to prevent potential attacks or resource exhaustion. Provide clear guidelines and documentation for users on the requirements and best practices for creating safe and compatible hook contracts.

#### Recommendations:

1. In the `PrizeVault` contract:
   - In the `depositWithPermit` function, consider validating the `_deadline` parameter to ensure it is not set too far in the future, which could lead to long-lasting permits that may be undesirable. Implement a maximum permit deadline limit to mitigate this risk.
   - In the `transferTokensOut` function, add checks to validate that the `_tokenOut` and `_amountOut` parameters are not zero. Revert if either of these parameters is invalid to prevent unintended behaviour.
   - In the `_depositAndMint` function, consider adding a check to ensure that the caller has approved the prize vault to spend the required amount of assets before initiating the deposit. This can help prevent unexpected reverts and improve user experience.
   - In the `_burnAndWithdraw` function, implement a check to ensure that the caller has sufficient balance to cover the requested withdrawal amount. Revert if the caller's balance is insufficient to prevent unexpected behaviour.

2. In the `PrizeVaultFactory` contract:
   - In the `deployVault` function, add checks to validate that the `_yieldFeePercentage` parameter is within a valid range (e.g., between 0 and 100). Revert if the provided fee percentage is invalid to prevent the creation of vaults with improper fee configurations.
   - Consider implementing a mechanism to set a maximum limit on the number of vaults that can be deployed by a single account. This can help prevent spam or abuse of the factory contract.
   - Add a check to ensure that the prize vault deployment succeeds before transferring the yield buffer to the newly deployed vault. This can help prevent loss of funds if the deployment fails for any reason.

3. In the `TwabERC20` contract:
   - In the constructor, consider adding a check to ensure that the provided `name_` and `symbol_` parameters are not empty strings. Revert if either of these parameters is invalid to prevent the creation of tokens with improper configurations.
   - In the `_transfer`, `_mint`, and `_burn` functions, add checks to validate that the `_amount` parameter is not zero. Revert if the amount is invalid to prevent unintended behavior.
   - Consider implementing a mechanism to enforce a maximum balance limit per account to mitigate potential risks associated with large token balances.

4. In the `Claimable` contract:
   - In the `claimPrize` function, add checks to validate that the `_tier`, `_prizeIndex`, and `_reward` parameters are within valid ranges based on the specific prize pool configuration. Revert if any of these parameters are invalid to prevent unexpected behavior.
   - Consider implementing a mechanism to limit the frequency at which a user can claim prizes to prevent potential abuse or excessive gas consumption.
   - Add a check to ensure that the prize payout succeeds before emitting the `PrizeClaimed` event. This can help maintain consistency between the contract state and emitted events.

5. In the `HookManager` contract:
   - In the `setHooks` function, consider implementing a maximum limit on the number of hooks that can be set per user to prevent potential storage exhaustion attacks.
   - Add a check to ensure that the provided hook implementation contract adheres to the required interface (e.g., by using `type(IVaultHooks).interfaceId`). Revert if the provided contract does not implement the necessary functions to prevent unexpected behavior.
   - Consider implementing a mechanism to allow users to remove previously set hooks to provide flexibility and control over their hook configurations.

6. General recommendations:
   - Implement comprehensive input validation for all external and public functions to ensure that the provided parameters are within expected ranges and adhere to any specific constraints. Revert with clear error messages if the input validation fails.

   - Consider using a secure randomness source, such as Chainlink VRF, for any randomization requirements within the prize vault ecosystem to ensure fairness and prevent potential manipulation.

 - Regularly monitor the prize vault ecosystem for any suspicious activities, anomalies, or unexpected behaviour, leveraging tools like [Tenderly](https://dashboard.tenderly.co/) and [OpenZeppelin Defender](https://defender.openzeppelin.com/) for real-time alerts and automated responses.

- Continuously stay updated with the latest security best practices, vulnerabilities, and attack vectors in the smart contract ecosystem, and incorporate relevant security enhancements and patches into the prize vault contracts as needed.

 - Establish a clear incident response plan and communication channels to promptly address and resolve any identified vulnerabilities or security issues. Regularly update and test the incident response procedures to ensure effectiveness. Give mention to Security Alliance's [SEAL-911](https://securityalliance.org/) to increase incident response time, and also build more trust with users.

#### Contract Details:

- Here are function interaction graphs for the key contracts. These graphs helped me in understanding the flow of data and identifying potential dependencies or vulnerabilities: 

  - `PrizeVault` contract:
    - Deposit flow: `deposit` -> `_depositAndMint` -> `yieldVault.mint` -> `_mint`
    - Withdrawal flow: `withdraw` -> `_burnAndWithdraw` -> `_burn` -> `yieldVault.redeem` -> `_asset.transfer`
    - Liquidation flow: `transferTokensOut` -> `_withdraw` -> `yieldVault.redeem` -> `_asset.transfer`

  - `PrizeVaultFactory` contract:
    - Deployment flow: `deployVault` -> `PrizeVault.constructor` -> `_asset.transferFrom`

  - `TwabERC20` contract:
    - Transfer flow: `transfer` -> `_transfer` -> `twabController.transfer`
    - Mint flow: `_mint` -> `twabController.mint`
    - Burn flow: `_burn` -> `twabController.burn`

  - `Claimable` contract:
    - Claim flow: `claimPrize` -> `_hooks[_winner].beforeClaimPrize` -> `prizePool.claimPrize` -> `_hooks[_winner].afterClaimPrize`

  - `HookManager` contract:
    - Set hooks flow: `setHooks` -> `_hooks[msg.sender] = hooks`

#### Conclusion:

The [PoolTogetherV5](https://github.com/code-423n4/2024-03-pooltogether) prize vault contracts exhibit a good design and adhere to best practices in many aspects. The use of modular contracts, access control mechanisms, and event emissions demonstrate a thoughtful approach to contract development. The integration with the ``TwabController`` for efficient balance tracking and the flexibility provided by the hook mechanism are notable strengths of the system.

In conclusion, while the [PoolTogetherV5](https://github.com/code-423n4/2024-03-pooltogether) prize vault contracts provide a solid foundation for a "no loss" prize savings system, the potential risks associated with the ecosystem necessitate continuous vigilance, as always. By addressing the identified concerns and implementing the recommended safeguards and incident response plans, the prize vault ecosystem can offer users a trust-less, long term and secure platform for participating in prize-linked savings.

### Time spent:
20 hours