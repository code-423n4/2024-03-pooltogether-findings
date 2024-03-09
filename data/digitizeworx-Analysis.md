## Overview

The core functionality of the PoolTogether V5 Prize Vault system revolves around the following key components:

1. **Asset Deposits**: Users can deposit their supported assets (such as stablecoins or other ERC20 tokens) into the Prize Vault contract. In return, they receive vault shares represented by the `TwabERC20` token, which serves as a receipt for their deposited assets.

2. **Yield Generation**: The deposited assets are automatically invested into integrated yield sources, such as decentralized lending protocols or yield aggregators. These yield sources generate returns on the deposited assets over time, increasing the total value locked in the Prize Vault.

3. **Prize Distribution**: The accrued yields are periodically harvested and distributed as prizes to lucky winners. The prize distribution mechanism is based on a configurable formula that determines the portion of the yields that are allocated as prizes and the portion that is retained as a reserve.

4. **Prize Claiming**: Winners can claim their prizes through the Prize Vault contract, which interacts with the `PrizePool` contract to transfer the prize amounts to the winners' accounts. The system also supports user-defined claim hooks, allowing winners to customize their prize claiming experience and integrate with external contracts or systems.

5. **Withdrawals**: Users can withdraw their deposited assets from the Prize Vault at any time by burning their vault shares. The contract calculates the user's proportional share of the total assets and transfers the corresponding amount back to their account.

## Contract Architecture

The PoolTogether V5 Prize Vault system is implemented through a set of interconnected smart contracts, each serving a specific purpose:

- `PrizeVault.sol`: The main contract that handles asset deposits, withdrawals, yield accrual, and prize distribution. It integrates with external yield sources and the `PrizePool` contract.

- `TwabERC20.sol`: An ERC20-compliant token contract representing the vault shares. It integrates with the `TwabController` contract for efficient balance tracking.

- `Claimable.sol`: An abstract contract providing an interface for prize claiming and supporting user-defined claim hooks.

- `HookManager.sol`: A contract for managing user-defined prize claim hooks, allowing users to customize their prize claiming experience.

- `PrizeVaultFactory.sol`: A factory contract for deploying new instances of the `PrizeVault` contract with specific configurations.

These contracts are designed to be modular, extensible, and compatible with various external systems and protocols. They follow best practices for smart contract development, such as using well-tested libraries, implementing access control mechanisms, and including fail-safe and emergency mechanisms.

## Security and Auditing

The PoolTogether V5 Prize Vault system undergoes rigorous security audits and testing to ensure its integrity and protection against common vulnerabilities and attack vectors. The contracts are thoroughly reviewed for potential security risks, such as reentrancy attacks, integer overflows/underflows, and access control issues.

However, it is important to note that the security and performance of the Prize Vault system also depend on the integrity and correctness of the integrated external components, such as yield sources and the `PrizePool` contract.

### Attacking the PrizeVault through the yield source integrations.

**Yearn Vaults:**
- Vault share price manipulation 
  - If an attacker can manipulate the Yearn vault share price (e.g. by exploiting Yearn's price oracle), they could trick the PrizeVault into minting more shares than assets truly deposited.
- Fake vault balance reporting
  - Yearn vaults rely on strategies that deposit into other protocols. If a strategy falsely reports inflated balances, the PrizeVault would think it has more assets than reality. An attacker could withdraw more than their fair share.
- Malicious strategy
  - A malicious or vulnerable Yearn strategy could potentially steal assets, making portions of the PrizeVault balance inaccessible and breaking accounting.

**Beefy Vaults:**
- Mootoken inflation attack
  - Beefy vaults use "mootokens" to represent shares. If there's a bug allowing arbitrary mootoken minting, an attacker could inflate the PrizeVault's balance and withdraw more underlying assets than they deserve. 
- Reward token hijacking
  - Many Beefy vaults auto-compound reward tokens. If an attacker can manipulate the reward token price or routing, they may be able to siphon off yield before it reaches the PrizeVault.

**sDAI and Element TVL depletion:**
- E.g. sDAI and Element are based on lending liquidity across long timeframes. If other lenders of that asset withdraw, the protocols could become undercollateralized. A naive integration might not account for this, allowing the last withdrawers to take more than their fair share from the PrizeVault.

**Generic:**
- Yield source hacked, portion of assets stolen
  - PrizeVault needs graceful handling if yield source is hacked and a material portion of assets are no longer accessible. It should freeze deposits, only allow proportional withdrawals, and clearly report the loss.
- Temporary asset freezing in yield source 
  - Some yield sources include privileged roles that can temporarily freeze assets. PrizeVault should have contingency withdrawal mechanisms and not rely on rebase optics during this state.
- Severe impairment, yield source < 50% collateralized
  - PrizeVault should detect severe losses and put itself into a withdrawal-only "recovery mode" as you outlined. No more deposits, proportional withdrawals based on remaining assets.

**To mitigate these:**
- PrizeVault should verify yield source balances/prices from multiple sources if possible 
- Diversify deposits across multiple yield sources
- Only allow withdrawals up to PrizeVault's actual accessible balance in yield source
- Extensive testing of loss scenarios
- Monitoring and alerting for large PrizeVault balance changes
- Ability to quickly pause deposits and enter "recovery mode"


**TwabERC20 contract's adherence to the ERC20 standard and the PrizeVault's implementation of ERC4626 to identify any potential vulnerabilities or deviations that could enable exploits.**

**TwabERC20 and ERC20 Compliance:**

The TwabERC20 contract inherits from OpenZeppelin's ERC20 and ERC20Permit contracts, which provide standard implementations of the ERC20 interface. However, TwabERC20 overrides several key functions to use the TwabController for balance storage and management.

**Potential Vulnerabilities:**
1. `balanceOf()` override
   - This function retrieves the balance from the TwabController rather than the contract's own state.
   - Risk: If the TwabController's balance data is manipulated or inconsistent, it could return incorrect balances.

2. `totalSupply()` override
   - This function retrieves the total supply from the TwabController rather than the contract's own state.
   - Risk: If the TwabController's total supply data is manipulated or inconsistent, it could return an incorrect total supply.

3. `_mint()` override
   - This function mints new tokens by calling the TwabController's `mint()` function directly.
   - Risk: If the TwabController's `mint()` function is vulnerable to unauthorized minting, it could allow excessive token creation.

4. `_burn()` override
   - This function burns tokens by calling the TwabController's `burn()` function directly.
   - Risk: If the TwabController's `burn()` function is vulnerable to unauthorized burning, it could allow malicious token destruction.

5. `_transfer()` override
   - This function transfers tokens by calling the TwabController's `transfer()` function directly.
   - Risk: If the TwabController's `transfer()` function is vulnerable to double spending or unauthorized transfers, it could allow token theft.

**To mitigate these risks, it's crucial to ensure that the TwabController's balance management functions are secure and consistent. Any vulnerabilities in the TwabController could propagate to the TwabERC20 token.**

It's also important to thoroughly test the TwabERC20 contract's behavior in edge cases, such as minting or burning very large amounts, transferring to/from the zero address, and handling of allowances.

Fuzz testing and formal verification could help uncover any hidden vulnerabilities or deviations from the ERC20 standard.

**PrizeVault and ERC4626 Implementation:**

The PrizeVault contract implements the ERC4626 "Tokenized Vault Standard" interface, which defines functions for depositing, withdrawing, and managing shares and underlying assets.

**Potential Vulnerabilities:**
1. Deposit/Mint functions 
   - These functions allow users to deposit assets and receive vault shares.
   - Risk: If the share minting calculations are incorrect or can be manipulated, it could allow minting of excess shares.

2. Withdraw/Redeem functions 
   - These functions allow users to burn shares and withdraw underlying assets.
   - Risk: If the share burning or asset transfer calculations are incorrect, it could allow over-withdrawal of assets.

3. Asset/Share Conversion functions
   - These functions calculate the amount of assets for a given amount of shares, and vice versa.
   - Risk: If the conversion calculations are incorrect or can be manipulated, it could allow minting or redemption of incorrect asset/share amounts.

4. Balance and Accounting functions 
   - These functions track the vault's total assets, debt, and yield balances.
   - Risk: If the accounting calculations are incorrect or can be manipulated, it could allow over-minting of shares or over-withdrawal of assets.

**To mitigate these risks, the PrizeVault contract includes several checks and safeguards, such as:**
- Using SafeMath/SafeCast for arithmetic operations to prevent overflows and underflows.
- Checking for zero inputs and shares/assets alignment to prevent invalid mints or redeems.
- Updating balances and state before external calls to prevent reentrancy attacks.
- Using a yield buffer to absorb small losses and prevent the vault from becoming insolvent due to rounding errors.

However, it's still important to thoroughly test the vault's ERC4626 implementation, especially around edge cases and extreme values. Fuzz testing, code audits, and formal verification can help identify any hidden vulnerabilities or deviations from the standard.

Simulation of various attack scenarios, such as attempts to manipulate share/asset conversions or exploit rounding errors, can also help validate the vault's security.

In summary, while the TwabERC20 and PrizeVault contracts leverage established standards (ERC20 and ERC4626), their customized implementations introduce potential vulnerabilities that must be carefully reviewed and tested.

The TwabERC20 contract's reliance on the TwabController for balance management means that any vulnerabilities in the controller could impact the token's security and compliance with ERC20.

Similarly, the PrizeVault's complex accounting and share/asset conversion logic could potentially be exploited if not implemented correctly.


PrizeVault and PrizeVaultFactory contracts for adherence to best practices, including error handling, input validation, and gas optimization.
----------------------------------

**Error Handling:**
The contracts make use of custom errors instead of plain string error messages, which is a good practice. Custom errors provide more descriptive and gas-efficient error handling. For example:

```solidity
/// @notice Thrown when the Yield Vault is set to the zero address.
error YieldVaultZeroAddress();

/// @notice Thrown when the Owner is set to the zero address.
error OwnerZeroAddress();
```

The contracts consistently use these custom errors throughout the codebase, enhancing readability and reducing gas costs.

**Input Validation:**
The contracts perform input validation in various places to ensure the integrity of the data and prevent unexpected behavior. For example:

```solidity
if (address(yieldVault_) == address(0)) revert YieldVaultZeroAddress();
if (owner_ == address(0)) revert OwnerZeroAddress();
```

These checks validate that the provided addresses are not zero addresses, preventing potential issues.

Similarly, the `setYieldFeePercentage` function validates that the input percentage does not exceed the maximum allowed value:

```solidity
if (_yieldFeePercentage > MAX_YIELD_FEE) {
    revert YieldFeePercentageExceedsMax(_yieldFeePercentage, MAX_YIELD_FEE);
}
```

However, there might be room for improvement in input validation. For example, the contracts could benefit from additional checks on input values, such as ensuring that certain parameters are within expected ranges or that array lengths match the expected sizes.

**Gas Optimization:**
The contracts demonstrate several gas optimization techniques:

1. Use of the `immutable` keyword for variables that are set once in the constructor and never modified afterward. This allows the compiler to replace variable references with their actual values, saving gas. For example:

```solidity
IERC4626 public immutable yieldVault;
```

2. Use of custom errors instead of string error messages, as mentioned earlier, which saves gas.

3. Use of the `unchecked` block to avoid unnecessary underflow/overflow checks when the operation is guaranteed to be safe. For example:

```solidity
unchecked {
    return type(uint96).max - _totalSupply;
}
```

However, there might be opportunities for further gas optimization. For instance, the contracts could explore the use of more efficient data structures or algorithms for certain operations, or they could consider caching frequently accessed values to reduce repeated computations.

**Code Refactoring and Simplification:**
The contracts are generally well-structured and follow a modular approach. The use of abstract contracts like `TwabERC20` and `Claimable` promotes code reusability and separation of concerns.

However, there are a few areas where the code could potentially be refactored or simplified:

1. Some functions, such as `_depositAndMint` and `_burnAndWithdraw`, have a relatively high level of complexity. Consider breaking them down into smaller, more focused functions to improve readability and maintainability.

2. The `_withdraw` function has a nested `if` statement that could be flattened to enhance readability.

3. The use of named return variables in some functions, such as `maxDeposit` and `maxMint`, could be replaced with direct returns to simplify the code.

4. Consider adding more comments or documentation to explain complex parts of the code or to clarify the purpose of certain variables or functions.

**Other Improvements:**
1. Consider implementing a more flexible access control system, such as role-based access control (RBAC), to manage permissions for different types of users or contracts interacting with the PrizeVault.

2. Explore the possibility of incorporating additional security mechanisms, such as contract pausability or emergency stop functionality, to handle unforeseen circumstances or vulnerabilities.

3. Conduct thorough unit testing and integration testing to ensure the correctness and robustness of the contract functionalities.

## Governance and Community

The PoolTogether V5 Prize Vault system is governed by a decentralized community of stakeholders, including users, developers, and token holders. Governance mechanisms, such as voting and proposals, may be implemented to enable community-driven decision-making and protocol upgrades.

The PoolTogether community actively participates in the development, testing, and promotion of the Prize Vault system. The project maintains an open-source codebase, encouraging contributions and collaboration from the wider blockchain community.

## Conclusion

The PoolTogether V5 Prize Vault smart contract system represents a significant advancement in decentralized prize-linked savings and yield generation. By leveraging the transparency, security, and immutability of the Ethereum blockchain, the system provides users with a novel and accessible way to save, earn yields, and have a chance to win prizes.

The modular and extensible architecture of the Prize Vault system allows for seamless integration with various yield sources and external protocols, enabling a diverse and robust ecosystem of prize-linked savings opportunities.

As the system continues to evolve and mature, it has the potential to revolutionize the way people think about savings and investments, making financial services more inclusive, transparent, and rewarding.

### Time spent:
17 hours