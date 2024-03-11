
# PoolTogether contest Analysis:

## System Overview

The PoolTogether system employs a sophisticated architecture to manage user interactions effectively. At its core, the PrizeVault contract is pivotal, handling user deposits, yield generation, and prize distribution. This contract leverages an underlying yield vault to generate yield, which is then converted into prize tokens, enhancing the overall user experience.

Key components of the system include the integration of OpenZeppelin contracts for ERC20 token functionality, SafeERC20 for safe ERC20 operations, and Ownable for access control. Additionally, it integrates with PoolTogether's TwabERC20, Claimable, and ILiquidationSource interfaces, demonstrating a robust and flexible design.

When adding a new vault to the system, the PrizeVaultFactory simplifies the deployment process. This factory contract is designed to create new prize vaults, each associated with a specific underlying yield vault. This approach facilitates the creation of multiple prize vaults, each designed to generate yield and distribute prizes to users efficiently.

The TwabERC20 contract plays a crucial role in creating an ERC20 token with balances stored in a TwabController, enabling time-weighted average balances for each depositor. This design is particularly important for maintaining accurate balance records in decentralized finance (DeFi) applications, especially in yield farming and staking scenarios where balances can fluctuate over time.

To automatically enable prize claims, the PoolTogether system utilizes the Claimable contract. This contract extends the functionality of vaults in PoolTogether V5 to enable automatic prize claims using an external claimer. This extension allows each account to set and manage prize hooks that are called when they win, significantly enhancing the user experience by automating the claiming process.

Lastly, the HookManager contract is used to manage user prize hooks on a vault. This functionality is crucial for enhancing the user experience by allowing custom actions to be executed when a user wins a prize, such as minting NFTs or executing complex logic. The contract uses a mapping to associate user addresses with their custom hooks, which are defined by the VaultHooks interface, enabling a flexible and personalized approach to prize claims.

## Admin Role and Ability

1. Ownership: The prizeVault contract uses the Ownable contract from OpenZeppelin, which provides a clear ownership model. The owner has the ability to perform administrative tasks such as setting the yield fee percentage and the yield fee recipient, which are crucial for managing the contract's operation.
2. Liquidation: The system allows for the liquidation of yield to generate prize tokens. This functionality is critical for maintaining the prize pool's balance and ensuring that prize tokens can be distributed to users.
3. Claimer Role: The contract defines a claimer address that has the exclusive ability to claim prizes on behalf of winners. This role is critical for automating the claim process and is set during the contract's deployment.
4. Hook Management: Users can set custom hooks for before and after prize claims, allowing for additional logic to be executed during the claim process. This flexibility enhances the contract's functionality but also introduces potential for misuse if not managed properly.

## Approach Taken to the Code Base

1. Modularity: The system is modular, utilizing external interfaces and contracts for functionality like yield generation (IERC4626), token operations (SafeERC20, ERC20), and liquidation (ILiquidationSource). This approach enhances security and maintainability.
2. Security Practices: The prizeVault contract uses SafeERC20 for safe operations on ERC20 tokens, which helps prevent common vulnerabilities associated with ERC20 token interactions.
3. Yield and Rounding Errors: The prizeVault contract employs strategies to mitigate rounding errors, ensuring that depositors can withdraw their full initial deposit amount without loss. This is a critical aspect of maintaining the contract's "no loss" principle.
Centralization Risk
4. Yield Vault Dependency: The PrizeVaultFactory contract relies on an underlying yield vault (IERC4626) to generate yield. The risk of centralization comes from this dependency. If the yield vault is compromised or fails, it could affect the contract's ability to generate yield and distribute prizes.
5. Salted CREATE2 Deployments: The use of a salted CREATE2 deployment (keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))) ensures the uniqueness of each deployed contract, which is crucial for avoiding collisions and replay attacks in the Ethereum ecosystem.

```solidity
file:  2024-03-pooltogether-main/pt-v5-vault/src/PrizeVaultFactory.sol
103    salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))

```

## Centralization Risk and Systemic Risk:

### Centralization Risk:

1. Admin Privileges: The owner has significant control over the contract's operation, including setting the yield fee percentage and the yield fee recipient. This concentration of power could be seen as a risk, although it's mitigated by the modular design and the use of OpenZeppelin contracts.
2. Yield Vault Dependency: in the prizeVault contract relies on an underlying yield vault (IERC4626) to generate yield. The risk of centralization comes from this dependency. If the yield vault is compromised or fails, it could affect the contract's ability to generate yield and distribute prizes.
3. Factory Ownership: While the factory contract itself does not have an explicit admin role, the deployment of new vaults and the management of yield buffers involve administrative tasks. The ability to control these aspects could be seen as a form of centralization risk, although it's mitigated by the use of external contracts and the nonce system for unique deployments.
4. Token Controller Management: The management of the TwabController could be seen as a form of centralization risk, especially if the controller is under the control of a single entity or group.

### Systemic Risk:

1. Liquidation and Yield Generation:   The process of liquidating yield and converting it into prize tokens introduces systemic risk. If the liquidation process fails or if the yield generation rate is insufficient, the prize pool might not have enough funds to distribute prizes.
2. Rounding Errors: The prizeVault contract's strategy to mitigate rounding errors introduces complexity and potential for miscalculations. While the contract attempts to minimize these errors, the possibility of errors remains.
Recommendations
3. Rounding Errors and Yield Buffer: The PrizeVaultFactory contract's approach to managing rounding errors and the yield buffer introduces systemic risk. If the yield buffer is depleted, the vault will prevent further deposits that would result in a rounding error, potentially affecting the vault's ability to generate yield and distribute prizes.
4. Total Supply Limitation: The limitation of balances and total supply to uint96 introduces a systemic risk, as it restricts the token's scalability and total supply. This could limit the token's utility in scenarios requiring high liquidity or large balances.
5. Gas Allocation for Hooks: The fixed gas allocation for hooks (HOOK_GAS) could be insufficient for complex hook logic, potentially leading to failed transactions and reduced functionality.


## Gas Optimization

| Number | Issue | Instences |
|--------|-------|-----------|
|[G-01]| State variables can be packed into fewer storage slots | 1 |
|[G-02]| Mappings not used externally/internally can be marked private | 2 |
|[G-03]| Low level call can be optimized with assembly | 3 |
|[G-04]| Using bytes32 is cheaper than using string.  | 8 |
|[G-05]| Check Arguments Early | 2 |
|[G-06]| State variables that could be declared constant | 6 |

## Recommendations

1. Audit and Testing: Given the complexity of the contracts, including its reliance on external contracts and the potential for rounding errors, thorough auditing and testing are essential.
2. Monitoring and Alerts: Implement monitoring for the underlying yield vault and the liquidation process. Setting up alerts for significant changes or failures in these areas can help mitigate risks associated with centralization and systemic issues.
3. Regular Updates:  Keep the system updated with the latest security practices and consider implementing additional safeguards against centralization and systemic risks.
In conclusion, the PoolTogether V5 Prize Vault contract is well-designed with a focus on security and fairness, leveraging established contracts and interfaces from the Ethereum ecosystem. However, the centralization risk associated with its dependency on an external yield vault and the complexity of managing rounding errors and yield generation highlight areas for careful monitoring and potential improvements.
4. Gas Optimization: Review and optimize the gas allocation for hooks to ensure that they can execute complex logic efficiently without risking failed transactions.

## Time Spent:

13 hours

### Time spent:
9 hours