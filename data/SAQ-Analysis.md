## Summary

no | File |
|-|:-|
| [[File-1](#file-1)] | PrizeVault.sol |
| [[File-2](#file-2)] | PrizeVaultFactory.sol | 
| [[File-3](#file-3)] | TwabERC20.sol | 
| [[File-4](#file-4)] | Claimable.sol | 
| [[File-5](#file-5)] | HookManager.sol | 


## Analysis Issue Report 


### [File-1] PrizeVault.sol

#### The bellow issues related to the File ( Link )
[Github-Link](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol)

<details>
<summary>see instances</summary>



#### Admin Abuse Risks:

* **Permit Functionality**: The `depositWithPermit` function allows users to approve and deposit assets using a permit. However, the comment mentions a potential risk related to the lack of a receiver parameter in the permit, allowing potential misuse.

* **Access Controls**: The contract includes several functions with the `onlyOwner` modifier, indicating that certain actions can only be performed by the owner. Ensure that proper access controls are in place and appropriately tested



#### Systemic Risks:

* **TWAB Controller Dependency**: The contract relies on an external TWAB controller (`twabController`). Ensure that the integration with this controller is secure and well-audited, as it seems to impact critical functions like sponsorship.

* **Yield Fee Handling**: The contract implements a yield fee mechanism. Ensure that the fee calculations and distributions are well-tested to prevent any unintended consequences.


#### Technical Risks:

* **ERC-20 Approval Risks**: The contract uses ERC-20's `approve` function, and it is crucial to handle allowances securely to prevent potential vulnerabilities like front-running attacks.

* **Reentrancy Consideration**: The code mentions considerations related to reentrancy during asset transfers. Make sure that reentrancy risks are thoroughly mitigated, especially during ERC-777 and ERC-20 transfers.


#### Integration Risks:

* **External Contracts**: The contract interacts with external contracts, such as `yieldVault` and `prizePool`. Ensure that these contracts are trustworthy and correctly integrated, considering potential vulnerabilities.

* **Liquidation Logic**: The `ILiquidationSource` interface and liquidation-related functions introduce dependencies on external systems. Assess and test the integration with these systems to avoid unforeseen issues.


#### Non-Standard Token Risks:

* **ERC-777 Considerations**: The contract mentions handling ERC-777 tokens and notes potential reentrancy issues. Ensure the contract correctly handles ERC-777 tokens, considering their unique features.

* **Permit Functionality:** The use of permit functionality may be non-standard, depending on the specific ERC-20 implementation. Ensure compatibility with popular ERC-20 token standards.


</details>


### [File-2] PrizeVaultFactory.sol

#### The bellow issues related to the File ( Link )
[Github-Link](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol)


<details>
<summary>see instances</summary>



#### Admin Abuse Risks:

* **Nonce Handling**: The contract utilizes nonces for address-based salts during vault deployment (`keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))`). While nonces generally prevent replay attacks, ensure that the nonce management is secure and doesn't introduce vulnerabilities.

* **Token Transfer**: The `deployVault` function transfers assets to the newly deployed vault to fill the yield buffer. Admins must be cautious about the management of these funds, and any vulnerability in this process could lead to potential abuse.


#### Systemic Risks:

* **Dependency on External Contracts**: The contract relies on external contracts (`PrizeVault`, `IERC4626`, `PrizePool`) for various functionalities. Ensure that these contracts are secure and well-audited, as any vulnerabilities in them could impact the overall system.

* **Relying on External Yield Vault**: The contract relies on an external ERC4626 yield vault (`_yieldVault`). Ensure that the integration with this yield vault is secure, and consider potential risks associated with the yield generation process.


#### Technical Risks:

* **CREATE2 Usage**: The contract uses `CREATE2` to deploy new PrizeVault instances. While this is a standard practice, ensure that the implementation is correct, and the risk of collisions is adequately mitigated.

* **Address Calculation for Salt**: The salt for `CREATE2` is generated using `keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))`. Validate that this salt calculation is robust and doesn't introduce any vulnerabilities.


#### Integration Risks:

* **ERC-4626 Integration**: The contract relies on an external ERC-4626 yield vault (`_yieldVault`). Ensure proper integration with this contract and assess potential risks associated with the yield generation process.

* **PrizePool Integration**: The contract interacts with an external `PrizePool` contract (`_prizePool`). Verify that the integration is secure and well-tested, considering potential implications for prize computations.


#### Non-Standard Token Risks:

* **Token Transfer**: The `deployVault` function involves the transfer of tokens (`IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER)`). Ensure that this token transfer is secure and won't result in any unexpected behavior.

**Yield Fee Handling**: The contract includes parameters related to yield fees (`_yieldFeeRecipient`, `_yieldFeePercentage`). Assess the implementation to ensure correct handling of yield fees and prevent any unintended consequences.

#### Recommendations:
1. **Security Audits**:
   * Conduct thorough security audits on external contracts (`PrizeVault`, `IERC4626`, `PrizePool`).
2. **Nonce Security**:
   * Implement secure nonce management to prevent replay attacks.
  
3. **Token Transfers**:
   * Review and test token transfer logic in the `deployVault` function for robustness.

4. **Yield Vault Integration**:
   * Ensure secure integration with the external ERC4626 yield vault.
5. **CREATE2 Usage**:
   * Verify correctness in using CREATE2 for deploying PrizeVault instances.
</details>


### [File-3] TwabERC20.sol

#### The bellow issues related to the File ( Link )
[Github-Link](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol)


<details>
<summary>see instances</summary>



#### Admin Abuse Risks:

* **No explicit admin controls**: The contract lacks explicit functions or modifiers for administrative actions. Admin abuse risks are mitigated by adhering to the standard ERC20 functions and relying on the TwabController.



#### Systemic Risks:

* **TwabController Dependency**: The contract is tightly coupled with the `TwabController`. Systemic risks may arise if the TwabController has vulnerabilities or malfunctions. Thoroughly audit the TwabController for robustness.


#### Technical Risks:

* **Gas Efficiency**: Gas efficiency is prioritized by using `uint96` for balances. Review potential gas implications and ensure minting doesn't exceed uint96 limits, causing failed transactions.


#### Integration Risks:

* **ERC20 Compliance**: The contract inherits from OpenZeppelin's ERC20 and ERC20Permit, ensuring standard ERC20 functionality. Integration risks are low, but thoroughly test interactions with external systems.


#### Non-Standard Token Risks:

* **TwabController Dependency**: The contract relies on a specific balance tracking mechanism (`TwabController`). Ensure compatibility with other systems, and assess risks associated with potential changes or upgrades to the TwabController.



#### Recommendations:

1. **TwabController Audit**:
   * Conduct a thorough audit of the TwabController to identify and address any vulnerabilities.

2. **Gas Efficiency Testing**:
   * Test the gas efficiency of the contract, especially minting functions, to ensure they stay within `uint96` limits.

3. **Integration Testing**:
   * Conduct extensive integration testing to verify seamless interaction with external systems, especially with the TwabController.

4. **Documentation**:
   * Provide clear documentation on the integration process and dependencies, especially regarding the TwabController.

5. **Security Best Practices**:
   * Follow security best practices in contract development and consider additional security measures if necessary.
  

</details>

### [File-4] Claimable.sol

#### The bellow issues related to the File ( Link )
[Github-Link](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol)


<details>
<summary>see instances</summary>



#### Admin Abuse Risks:

* **Claimer Authority**: Admin abuse risks are present due to the `claimer` variable, which determines the address allowed to claim prizes. Ensure that the ` ` address is set securely and that its authority is well-defined.



#### Systemic Risks:

* **PrizePool Dependency**: The contract is tightly coupled with the `PrizePool` contract. Systemic risks may arise if the PrizePool has vulnerabilities or malfunctions. A comprehensive audit of the PrizePool contract is crucial.


#### Technical Risks:

* **Hook Execution**: The contract implements hooks for before and after prize claim actions. Ensure that these hooks are implemented securely, as any issues in their execution can impact prize claims.


#### Integration Risks:

* **ClaimPrize Function**: The `claimPrize` function interacts with external systems, specifically the PrizePool. Integration risks may arise if the PrizePool contract behavior changes. Test thoroughly for seamless interaction.


#### Non-Standard Token Risks:

* **No Non-Standard Token Risks Detected**: The contract does not involve non-standard tokens.


#### Recommendations:

1. **Claimer Authentication**:
   * Implement secure mechanisms to set and manage the `claimer` address, potentially utilizing multi-signature schemes or time-lock mechanisms to minimize admin abuse risks.
2. **PrizePool Audit**:  
   * Conduct a thorough audit of the `PrizePool` contract to identify and address any vulnerabilities. Ensure that the PrizePool contract is robust and secure.
3. **Hook Implementation Review**:
   * Review the implementation of before and after claim hooks. Ensure that the hooks are securely designed and do not introduce vulnerabilities.
4. **Integration Testing**:
   * Test the contract's interaction with the PrizePool thoroughly. Simulate various scenarios, including potential changes in PrizePool behavior, to ensure smooth integration.
5. **Documentation**:
   * Provide clear documentation on the purpose and usage of the contract, including details on the `claimer` address and the hook mechanisms.
  

</details>

### [File-5] HookManager.sol

#### The bellow issues related to the File ( Link )
[Github-Link](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol)


<details>
<summary>see instances</summary>



#### Admin Abuse Risks:

* **SetHooks Functionality**: There's a potential risk of admin abuse as the `setHooks` function allows any account to set hooks for themselves. Ensure that only authorized users can set hooks to prevent misuse.


#### Systemic Risks:

* **Dependency on External Hooks**: The contract introduces a system where individual accounts can set hooks. Systemic risks may arise if the hook execution mechanism is not well-secured or if it introduces vulnerabilities.

#### Technical Risks:

* **Hook Storage**: Storing hooks in a mapping (_hooks) could lead to higher gas costs if the mapping becomes too large. Ensure that the impact on gas consumption is acceptable.


#### Integration Risks:

* **External Systems Interaction:** If the hooks interact with external systems, there is an integration risk. Ensure that the external systems are secure and that the interaction does not introduce vulnerabilities.


#### Non-Standard Token Risks:

* **No Non-Standard Token Risks Detected**: The contract does not involve non-standard tokens.


#### Recommendations:

1. **Access Control**:
   * Implement access control mechanisms to restrict who can call the `setHooks` function. This helps prevent unauthorized users from setting hooks.
2. **Security Audits**:
   * Conduct a security audit, especially if the hooks interact with external systems. Verify that the hooks and their execution are secure and do not expose the contract to vulnerabilities.
3. **Gas Consumption Consideration**:
   * Evaluate the potential impact on gas consumption due to the storage of hooks in a mapping. Consider gas-efficient alternatives if the mapping size becomes a concern.
4. **Documentation**:
   * Provide comprehensive documentation on the purpose of hooks, how users can set them, and any potential risks associated with hook execution.

</details>


### Time spent:
5 hours