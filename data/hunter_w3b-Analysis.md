# Analysis - PoolTogether Contest

![PoolTogether-Protocol](https://code4rena.com/_next/image?url=https%3A%2F%2Fstorage.googleapis.com%2Fcdn-c4-uploads-v0%2Fuploads%2FsgeA99L9Hpc.0&w=256&q=75)

## Description overview of `PoolTogether` Contest

PoolTogether is a decentralized prize savings protocol that allows users to deposit funds in a pool and earn yield without risking their principal. The yield earned is then distributed as prizes to depositors randomly. The protocol uses smart contracts to ensure transparency, security, and fairness.

**Key Features:**

- **No-loss lottery:** Users can never lose their principal.
- **Fair and transparent:** All prize drawings are conducted randomly, and the results are publicly verifiable.
- **Autonomous and permissionless:** Anyone can add new assets and yield sources to the protocol.
- **Supports multiple tokens:** All tokens share one big pool of prizes to make the prizes as large as possible.

**How PoolTogether Works:**

1. Users deposit tokens into a prize pool.
2. The prize pool earns yield on the deposited tokens.
3. The yield is distributed to winners in the form of random prizes.
4. Users can withdraw their deposits at any time.

**The PrizeVault Contract:**

The PrizeVault contract is a redesigned and refactored version of the previous Vault contract used in PoolTogether. It is designed to be fully compliant with the ERC4626 specification and interface cleanly with underlying yield vaults. The PrizeVault employs two strategies to cover rounding errors and ensure that depositors can withdraw every last wei of their initial deposit:

- **Dust collection strategy:** Mints yield vault shares directly to reduce the severity of rounding errors during deposit and withdrawal.
- **Yield buffer:** Ensures that there is always enough yield reserved to cover the rounding errors on deposits and withdrawals.

**PoolTogether V5:**

PoolTogether V5 is the latest version of the protocol and brings several improvements, including:

- **Autonomous:** There is no central entity controlling the protocol.
- **Permissionless:** Anyone can add new assets and yield sources to the protocol.
- **Adaptive prize count:** Adjusts the number of prize tiers based on how many prizes were claimed for the previous awarded draw.

**Vaults and Prize Claims:**

When users deposit tokens into PoolTogether, they are depositing into an ERC4626 vault that tracks deposits using the Twab Controller. Vaults claim prizes on behalf of their users, which gives Vaults flexibility in how they want to handle prizes. The default PoolTogether Vault has a separate Claimer contract through which users claim prizes for others.

## System Overview

### Scope

**Main contracts**

- **PrizeVault.sol:** This contract is an implementation of a `prize vault` for the PoolTogether V5 protocol. The prize vault is a contract that takes deposits of an asset, earns yield with the deposits through an underlying yield vault, and then contributes the yield to a prize pool as prize tokens. Depositors of the prize vault are then eligible to win prizes from the pool. The contract is designed to ensure that depositors can always withdraw their full deposit amount, even if the underlying yield source loses assets, by employing two strategies: the "`dust collection strategy`" and the "`yield buffer`". The dust collection strategy involves calculating the amount of yield vault shares that would be minted during a deposit and minting those shares directly, ensuring that only the exact amount of assets needed are sent to the yield vault while keeping the remainder as a latent balance in the prize vault until it can be used in the next deposit or withdraw. The yield buffer is a reserve of yield that is used to cover rounding errors on deposits and withdrawals, and is expected to be of insignificant value. The contract also includes functions for setting the yield fee percentage and yield fee recipient, as well as for claiming yield fee shares.

  - **Key Features:**

    - Deposit assets and earn yield through an underlying yield vault
    - Liquidate yield and contribute to the prize pool as prize tokens
    - Employ "dust collection strategy" and "yield buffer" to cover rounding errors and ensure full deposit withdrawal
    - Set yield fee percentage and recipient
    - Claim yield fee shares

  - **Functionalities:**

    - `deposit`: Deposit assets and earn yield
    - `withdraw`: Withdraw deposited assets
    - `redeem`: Redeem yield vault shares for assets
    - `depositWithPermit`: Deposit assets on behalf of another user using ERC20 Permit
    - `sponsor`: Delegate ownership of the vault to another address
    - `claimYieldFeeShares`: Claim yield fee shares
    - `setYieldFeePercentage`: Set the yield fee percentage
    - `setYieldFeeRecipient`: Set the yield fee recipient

  - **Security Measures:**

    - SafeERC20 library for safe token transfers
    - Math library for safe mathematical operations
    - Checks for zero addresses and invalid parameters
    - Limit on yield fee percentage
    - Reentrancy protection using "checks-effects-interactions" pattern

  - **Core Logic:**

    - Holds an underlying ERC4626 vault that generates yield
    - Liquidates yield and contributes to the prize pool as prize tokens
    - "`Dust collection strategy`" calculates and mints yield vault shares directly during deposits
    - "Yield buffer" reserve covers rounding errors on deposits and withdrawals

  - **Additional Features:**

    - Support for ERC20 Permit
    - Support for sponsoring the vault
    - Support for setting a liquidation pair contract
    - Support for setting a yield fee percentage and recipient, with the option to claim yield fee shares
    - Emits events for yield fee recipient and percentage changes, sponsor, and yield transfer out.

- **PrizeVaultFactory.sol:** The PrizeVaultFactory contract is a factory contract for deploying new prize vaults using a standard underlying ERC4626 yield vault. The factory contract maintains a list of all deployed vaults and allows the deployer to specify the name, symbol, yield vault, prize pool, claimer, yield fee recipient, yield fee percentage, and owner of the new vault.

  - **Key Features**

    - Deploys new prize vaults using a standard underlying ERC4626 yield vault.
    - Maintains a list of all deployed vaults.
    - Verifies if a vault has been deployed via this factory.
    - Stores deployer nonces for CREATE2.

  - **Functionalities**

    - `deployVault`: Deploys a new prize vault.
    - `totalVaults`: Returns the total number of vaults deployed by this factory.

  - **Security Measures**

    - Uses `CREATE2` to deploy vaults with predictable addresses.
    - Requires the caller to approve this factory to spend underlying assets equal to `YIELD_BUFFER` so the yield buffer can be filled on deployment.

  - **Core Logic**

    - The factory deploys new prize vaults using the `PrizeVault` contract. The `PrizeVault` contract is responsible for managing the underlying yield vault, distributing prizes, and claiming winnings.

    - The factory also maintains a list of all deployed vaults and a mapping to verify if a vault has been deployed via this factory. This allows the factory to track the number of vaults deployed and to prevent duplicate deployments.

  - **Additional Features**

    - The factory uses a `yield buffer` to cover rounding errors on deposits and withdrawals.
    - The factory allows the deployer to specify the name, symbol, yield vault, prize pool, claimer, yield fee recipient, yield fee percentage, and owner of the new vault.

- **TwabERC20.sol:** The TwabERC20 contract uses a `TwabController` to keep track of balances. This allows for `time-weighted` average balances for each depositor and token compatibility with the PoolTogether V5 Prize Pool.

  - **Key Features**

    - ERC20 token with balances stored in a `TwabController`.
    - Time-weighted average balances for each depositor.
    - Compatible with the PoolTogether V5 Prize Pool.

  - **Functionalities**

    - `balanceOf`: Returns the balance of an account.
    - `totalSupply`: Returns the total supply of tokens.
    - `mint`: Mints tokens to an account.
    - `burn`: Burns tokens from an account.
    - `transfer`: Transfers tokens from one account to another.

  - **Security Measures**

    - The `TwabController` limits all balances including total token supply to uint96 for gas savings. Any mints that increase a balance past this limit will fail.

  - **Core Logic**

    - The `TwabController` is used to keep track of balances. This allows for time-weighted average balances for each depositor. The TwabController also limits all balances including total token supply to uint96 for gas savings.

  - **Additional Features**

    - The contract is compatible with the PoolTogether V5 Prize Pool.

**abstract**

- **Claimable.sol:** The `Claimable` contract extension for PoolTogether V5. It provides an interface for Claimer contracts to interact with a vault in PoolTogether V5 while allowing each account to set and manage prize hooks that are called when they win.

  - **Key Features**

    - Provides an interface for Claimer contracts to interact with a vault in PoolTogether V5.
    - Allows each account to set and manage prize hooks that are called when they win.
    - Includes a `beforeClaimPrize` and `afterClaimPrize` hook that can be used to customize the prize claiming process.

  - **Functionalities**

    - `claimPrize`: Claims a prize for a winner.
    - `setClaimer`: Sets the claimer address.

  - **Core Logic**

    - The contract uses a `HookManager` to manage prize hooks. Each account can set their own prize hooks, which are called before and after a prize is claimed.

    - The `claimPrize` function claims a prize for a winner. It first calls the `beforeClaimPrize` hook for the winner, if set. Then, it claims the prize from the Prize Pool. Finally, it calls the `afterClaimPrize` hook for the winner, if set.

  - **Additional Features**

    - The contract includes a `HOOK_GAS` constant that specifies the amount of gas to give to each of the before and after prize claim hooks. This should be enough gas to mint an NFT if needed.

- **HookManager.sol:** The `HookManager` contract allows each account to set and manage prize hooks that can be called when they win.

  - **Key Features**

    - Allows each account to set and manage prize hooks.
    - Hooks can be called before and after a prize is claimed.
    - Hooks can be used to customize the prize claiming process.

  - **Functionalities**

    - `getHooks`: Gets the hooks for a given account.
    - `setHooks`: Sets the hooks for a winner.

  - **Core Logic**

    - The contract uses a mapping to store the hooks for each account. The `getHooks` function returns the hooks for a given account. The `setHooks` function sets the hooks for a winner.

**interfaces**

- **IVaultHooks.sol:** In this `IVaultHooks` interface. The VaultHooks struct defines a hook implementation and instructions on which hooks to call. The `IVaultHooks` interface defines two functions that can be implemented by contracts to handle prize claiming hooks.

### Roles

1. **PrizeVault.sol:**

   1. **Users/Depositors**: These are the individuals or entities who deposit assets into the Prize Vault to earn yield and become eligible for prizes.

   2. **Owner**: This is the address that has control over certain aspects of the contract, such as setting the yield fee recipient, yield fee percentage, and liquidation pair.

   3. **Yield Fee Recipient**: This is the address that receives the yield fee, which is a percentage of the yield generated by the assets in the Prize Vault.

   4. **Liquidation Pair**: This is the contract that is responsible for liquidating the yield generated by the assets in the Prize Vault into prize tokens.

   5. **Claimer**: This is the contract that is allowed to claim prizes on behalf of the users/depositors.

   6. **Yield Vault**: This is the underlying ERC4626 vault where the assets are deposited to generate yield.

   7. **Prize Pool**: This is the contract that computes the prizes that are distributed to the users/depositors.

   8. **ERC20 Token Contract**: This is the contract that represents the underlying asset that is being deposited into the Prize Vault.

   9. **TwabController**: This is the contract that manages the Time-Weighted Average Balance (TWAB) calculations for distributing prizes.

2. **PrizeVaultFactory.sol:**

   1. **Factory**: This is the main contract that deploys new Prize Vaults using a standard underlying ERC4626 yield vault. It stores a list of all vaults deployed by this factory and verifies if a vault has been deployed via this factory.

   2. **Deployer**: This is the address that deploys a new vault using the `deployVault` function. The deployer must approve this factory to spend underlying assets equal to `YIELD_BUFFER` so the yield buffer can be filled on deployment.

   3. **Prize Vault**: This is the contract that is deployed by the factory. It manages deposits, withdrawals, and yield generation for a specific underlying asset. It's represented by the `PrizeVault` variable.

   4. **Yield Vault**: This is an ERC4626 vault where the assets are deposited to generate yield. It's passed as a constructor argument to the `PrizeVault` contract.

   5. **Claimer**: This is an address that is allowed to claim prizes on behalf of others. It's passed as a constructor argument to the `PrizeVault` contract.

   6. **Yield Fee Recipient**: This is the address that receives the yield fee. It's passed as a constructor argument to the `PrizeVault` contract.

   7. **Owner**: This is the address that owns the Prize Vault and can control certain aspects of it. It's passed as a constructor argument to the `PrizeVault` contract.

3. **Claimable.sol:**

   1. **PrizePool**: This is a contract that computes prizes. It's passed as a constructor argument to the `Claimable` contract and represented by the `prizePool` variable.

   2. **Claimer**: This is the address that is allowed to claim prizes on behalf of others. It's passed as a constructor argument to the `Claimable` contract and represented by the `claimer` variable.

   3. **Winner**: This is the address that has won a prize. It's passed as a parameter to the `claimPrize` function.

   4. **Reward Recipient**: This is the address that receives the prize reward. It's passed as a parameter to the `claimPrize` function.

### Chain support

Mainnet Ethereum, Optimism

### Invariants Generated

#### Main invariants

The PrizeVault invariants are split into two categories:

- Invariants for normal operating conditions
- Invariants for when the underlying yield vault has lost funds (recovery mode)

**PrizeVault.sol**

- This invariants of the contract should always hold true, regardless of the state changes or function calls made to the contract.

  1.  The total assets controlled by the vault should always be greater than or equal to the total share supply: `if (totalAssets() < totalDebt()) revert LossyDeposit(totalAssets(), totalDebt());`
  2.  The total assets controlled by the vault should always be greater than or equal to the total asset debt owed: `if (totalDebt_ >= _totalAssets) { return 0; }`
  3.  The total yield balance should always be less than or equal to the total assets controlled by the vault: `if (totalYieldBalance_ >= _yieldBuffer) { unchecked { return totalYieldBalance_ - _yieldBuffer; } }`
  4.  The yield fee percentage should always be less than or equal to the maximum yield fee percentage: `if (_yieldFeePercentage > MAX_YIELD_FEE) { revert YieldFeePercentageExceedsMax(_yieldFeePercentage, MAX_YIELD_FEE); }`
  5.  The yield fee recipient should always be set to a non-zero address: `if (address(_yieldFeeRecipient) == address(0)) revert LPZeroAddress();`
  6.  The liquidation pair should always be set to a non-zero address: `if (address(_liquidationPair) == address(0)) revert LPZeroAddress();`
  7.  The total available yield should always be less than or equal to the total yield balance: `if (_amountOut + _yieldFee > _availableYield) { revert LiquidationExceedsAvailable(_amountOut + _yieldFee, _availableYield); }`
  8.  The total supply of shares minted should always be less than or equal to the maximum supply limit: `return twabSupplyLimit_ < _maxDeposit ? twabSupplyLimit_ : _maxDeposit;`
  9.  The yield vault address should always be set to a non-zero address: `if (address(yieldVault_) == address(0)) revert YieldVaultZeroAddress();`
  10. The owner address should always be set to a non-zero address: `if (owner_ == address(0)) revert OwnerZeroAddress();`

**PrizeVaultFactory.sol**

- This invariants of the contract should always hold true, regardless of the state changes or function calls made to the contract.

  1.  The yield buffer should always be set to a non-zero value: `uint256 public constant YIELD_BUFFER = 1e5;`
  2.  The yield buffer should always be less than or equal to the total assets controlled by the vault: `IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER);`
  3.  The total number of vaults deployed by the factory should always be equal to the length of the `allVaults` array: `allVaults.push(_vault);`
  4.  The `deployedVaults` mapping should always contain a true value for every address of a vault deployed by the factory: `deployedVaults[address(_vault)] = true;`
  5.  The `deployerNonces` mapping should always contain a nonce value for every deployer address: `keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))`
  6.  The `PrizeVault` contract should always be deployed with a valid `_yieldVault`, `_prizePool`, `_claimer`, `_yieldFeeRecipient`, `_yieldFeePercentage`, and `_owner` address:

      ```solidity
      PrizeVault _vault = new PrizeVault{
         salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))
      }(
         _name,
         _symbol,
         _yieldVault,
         _prizePool,
         _claimer,
         _yieldFeeRecipient,
         _yieldFeePercentage,
         YIELD_BUFFER,
         _owner
      );
      ```

  7.  The `_name` and `_symbol` parameters should always be non-empty strings:

      ```solidity
      PrizeVault _vault = new PrizeVault{
         salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))
      }(
         _name,
         _symbol,
         _yieldVault,
         _prizePool,
         _claimer,
         _yieldFeeRecipient,
         _yieldFeePercentage,
         YIELD_BUFFER,
         _owner
      );
      ```

  8.  The `_claimer` address can be set to address zero if none is available yet: `_claimer can be set to address zero if none is available yet.`
  9.  The `_yieldFeePercentage` should always be a non-negative value: `uint32 _yieldFeePercentage`

**Claimable.sol**

- This invariants of the contract should always hold true, regardless of the state changes or function calls made to the contract.

  1.  The `prizePool` address should always be a non-zero address: `if (address(prizePool_) == address(0)) revert PrizePoolZeroAddress();`
  2.  The `claimer` address should always be a non-zero address: `if (_claimer == address(0)) revert ClaimerZeroAddress();`
  3.  The `_winner` address should always be a non-zero address: `recipient = _winner;`
  4.  The `_rewardRecipient` address should always be a non-zero address: `prizePool.claimPrize(_winner, _tier, _prizeIndex, recipient, _reward, _rewardRecipient);`
  5.  The `claimPrize` function should only be called by the `claimer` address: `modifier onlyClaimer() { if (msg.sender != claimer) revert CallerNotClaimer(msg.sender, claimer); _; }`
  6.  The `prizeTotal` returned by the `prizePool.claimPrize` function should always be a non-negative value: `uint256 prizeTotal = prizePool.claimPrize(_winner, _tier, _prizeIndex, recipient, _reward, _rewardRecipient);`
  7.  The `HOOK_GAS` value should always be enough gas to mint an NFT if needed: `uint24 public constant HOOK_GAS = 150_000;`
  8.  The `_hooks[_winner].useBeforeClaimPrize` and `_hooks[_winner].useAfterClaimPrize` boolean values should always be correctly set and updated by the `HookManager` contract.
  9.  The `beforeClaimPrize` and `afterClaimPrize` functions of the `_hooks[_winner].implementation` contract should always be called with the correct arguments and with the correct amount of gas.

## Approach Taken-in Evaluating `PoolTogether` Protocol

Accordingly, I analyzed and audited the subject in the following steps;

1.  **Core Protocol Contract Overview**:

    I focused on thoroughly understanding the codebase and providing recommendations to improve its functionality.
    The main goal was to take a close look at the important contracts and how they work together in the `PoolTogether`.

    I start with the following contracts, which play crucial roles in the `PoolTogether`:

                  PrizeVault.sol
                  PrizeVaultFactory.sol
                  Claimable.sol

    I started my analysis by examining the intricate structure and functionalities of the `PoolTogether` protocol. The protocol is designed to allow users to deposit assets into a prize vault, which then earns yield through an underlying yield vault. The yield is then liquidated and contributed to a prize pool, which is used to award prizes to depositors. The protocol also allows for custom hooks to be set by depositors, which are called directly before and after their prize is claimed.

    The protocol consists of several contracts, including the `PrizeVault`, `Claimable`, `TwabERC20`, `PrizePool`, and `TwabController` contracts. The `PrizeVault` contract is the main contract that handles deposits, withdrawals, and liquidation of yield. The `Claimable` contract provides an interface for claimer contracts to interact with the prize vault and set and manage prize hooks. The `TwabERC20` contract is an ERC20 token that is used to represent shares in the prize vault. The `PrizePool` contract is responsible for computing prizes and awarding them to winners. The `TwabController` contract is responsible for tracking time-weighted average balances of users' deposits.

    The protocol employs several strategies to ensure that depositors can withdraw their full deposit amount and no more, even in the event of losses in the underlying yield vault. The `PrizeVault` contract uses a "dust collection strategy" and a "yield buffer" to cover rounding errors on deposits and withdrawals. The protocol also prevents deposits if the yield buffer is depleted, and enters a "lossy withdrawal state" where depositors will incur rounding errors on withdraw.

    The protocol also includes several security features, such as the use of the `SafeERC20` library, the `Ownable` contract for controlling contract ownership, and the `onlyClaimer` modifier to restrict certain functions to the prize claimer contract.

2.  **Documentation Review**:

    Then went to Review these documentations[1](https://dev.pooltogether.com/),[2](https://docs.pooltogether.com/welcome/master),[3](https://docs.cabana.fi/) for a more detailed and technical explanation of the `PoolTogether` aslo have a good Walkthrough Video for explain Prize Vault codebase[link](https://ipfs.io/ipfs/bafybeigjlvmcieuc3j7lypxg2653jaxukbnsbj44kcvqsukfyt6sgskhgm).

3.  **Compiling code and running provided tests**:

4.  **Manuel Code Review** In this phase, I initially conducted a line-by-line analysis, following that, I engaged in a comparison mode.

    - **Line by Line Analysis**: Pay close attention to the contract's intended functionality and compare it with its actual behavior on a line-by-line basis.

    - **Comparison Mode**: Compare the implementation of each function with established standards or existing implementations, focusing on the function names to identify any deviations.

## Codebase Quality

Overall, I consider the quality of the `PoolTogether` protocol codebase to be Good. The code appears to be mature and well-developed. We have noticed the implementation of various standards adhere to appropriately. Details are explained below:

| Codebase Quality Categories              | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Architecture & Design**                | The protocol features a modular design, segregating functionality into distinct contracts (e.g., `PrizeVault`, `PrizeVaultFactory`, `TwabERC20`, `Claimable`) for clarity and ease of maintenance.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| **Error Handling & Input Validation**    | Functions check for conditions and validate inputs to prevent invalid operations, though the depth of validation (e.g., for edge cases transactions) would benefit from closer examination.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **Code Maintainability and Reliability** | The contracts are written with emphasis on sustainability and simplicity. The functions are single-purpose with little branching and low cyclomatic complexity. The protocol includes a novel mechanism for collecting fees that offers an incentive for this work to be done by outside actors,thereby removing the associated complexity.                                                                                                                                                                                                                                                                                                                                                             |
| **Code Comments**                        | The contracts are accompanied by comprehensive comments, facilitating an understanding of the functional logic and critical operations within the code. Functions are described purposefully, and complex sections are elucidated with comments to guide readers through the logic. Despite this, certain areas, particularly those involving intricate mechanics or tokenomics, could benefit from even more detailed commentary to ensure clarity and ease of understanding for developers new to the project or those auditing the code.                                                                                                                                                             |
| **Testing**                              | The contracts exhibit a commendable level of test coverage, which is indicative of a robust testing regime. This coverage ensures that a wide array of functionalities and edge cases are tested, contributing to the reliability and security of the code. However, to further enhance the testing framework, the incorporation of fuzz testing and invariant testing is recommended. These testing methodologies can uncover deeper, systemic issues by simulating extreme conditions and verifying the invariants of the contract logic, thereby fortifying the codebase against unforeseen vulnerabilities.                                                                                         |
| **Code Structure and Formatting**        | The codebase benefits from a consistent structure and formatting, adhering to the stylistic conventions and best practices of Solidity programming. Logical grouping of functions and adherence to naming conventions contribute significantly to the readability and navigability of the code. While the current structure supports clarity, further modularization and separation of concerns could be achieved by breaking down complex contracts into smaller, more focused components. This approach would not only simplify individual contract logic but also facilitate easier updates and maintenance.                                                                                         |
| **Strengths**                            | The `PoolTogether` protocol has a well-designed architecture with robust security features, such as the use of the SafeERC20 library, the Ownable contract for controlling contract ownership, and the onlyClaimer modifier to restrict certain functions to the prize claimer contract, along with a "dust collection strategy" and a "yield buffer" to cover rounding errors on deposits and withdrawals.                                                                                                                                                                                                                                                                                             |
| **Documentation**                        | The NatSpec is mostly complete for all external functions,and there are helpful inline comments throughout. However, there currently is no external documentation for users or integrators. Additionally, some user-facing documentation does not identify the risks and nuances of the staking contract, which is important. The technical developer documentation for the `PoolTogether` protocol provides clear and detailed explanations of the various smart contracts and their functionalities, including the PrizeVault, `Claimable`, `TwabERC20`, `PrizePool`, and `TwabController` contracts, along with code examples and explanations of the protocol's architecture and security features. |

## Architecture

### **System Workflow**

The PoolTogether protocol is a DeFi platform that enables users to pool their assets together and earn yield through an underlying yield vault. The yield generated by the yield vault is then used to fund prizes, which are distributed to depositors in a no-loss lottery. The protocol is designed to ensure that depositors can always withdraw their full deposit amount, even if the underlying yield source loses assets. It employs two strategies to cover rounding errors on deposits and withdrawals: the "dust collection strategy" and the "yield buffer". The dust collection strategy involves minting yield vault shares directly and keeping the remainder as a latent balance in the prize vault until it can be used in the next deposit or withdraw. The yield buffer is a reserve of yield that is used to cover rounding errors on deposits and withdrawals. If the yield buffer is depleted, new deposits will be prevented and the prize vault will enter a lossy withdrawal state. The protocol supports ERC4626 yield vaults that do not take a fee on deposit or withdraw.

| File Name               | Core Functionality                                                                                                                                                                                                                                                                                                                                                      | Technical Characteristics                                                                                                                                                                                    | Importance and Management                                                                                                                                                                                                                                                                                                                                                                                                          |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `PrizeVault.sol`        | The Prize Vault takes deposits of an asset and earns yield with the deposits through an underlying yield vault. The yield is then liquidated and contributed to the prize pool as prize tokens. Depositors are eligible to win prizes from the pool and if a prize is won, the permitted claimer contract for the prize vault claims the prize on behalf of the winner. | The contract uses `ERC4626` standard for tokenized vaults, employs a "`dust collection strategy`" and a "yield buffer" to cover rounding errors, and supports deposit, withdraw, mint, and redeem functions. | The Prize Vault is a crucial component of the `PoolTogether V5` protocol as it manages deposits, yield generation, and prize distribution and the contract is managed by an Ownable contract, which allows the owner to set the yield fee recipient, yield fee percentage, and liquidation pair.                                                                                                                                   |
| `PrizeVaultFactory.sol` | The core functionality of the `PrizeVaultFactory` contract is to deploy PrizeVault contracts using an ERC4626 yield vault and contributing to a PrizePool.                                                                                                                                                                                                              | Technical characteristics include utilizing the `deployVault` function to create new PrizeVault instances and managing the yield buffer to cover rounding errors on deposits and withdrawals.                | It's important to maintain the `yield buffer` at a level considered insignificant to ensure efficient operation and prevent potential loss due to rounding errors.Management involves tracking deployed vaults and nonces, ensuring the integrity of the deployment process and overall system functionality.                                                                                                                      |
| `TwabERC20.sol`         | Creating an ERC20 token with balances stored in a `TwabController` for time-weighted average balance calculation.                                                                                                                                                                                                                                                       | Utilizes ERC20 and ERC20Permit from OpenZeppelin, with SafeCast for safe type conversions, and integrates with TwabController for balance management.                                                        | Crucial for enabling `time-weighted` average balances and compatibility with PoolTogether V5 Prize Pool, ensuring efficient gas usage through uint96 limits. Involves careful balance tracking and supply management through interactions with the `TwabController`, ensuring integrity and accuracy of token balances.                                                                                                            |
| `Claimable.sol`         | The core functionality of the `Claimable` contract is to provide an interface for Claimer contracts to interact with a vault in PoolTogether V5 while allowing each account to set and manage prize hooks that are called when they win.                                                                                                                                | Utilizes `HookManager` and implements the `IClaimable` interface, defining gas allocation for before and after prize claim hooks, and integrates with PrizePool and external interfaces.                     | Crucial for enabling decentralized prize `claiming` functionality in `PoolTogether V5`, ensuring fair and customizable prize distribution mechanisms through prize hooks managed by individual claimants and Involves careful management of claimer addresses, handling the execution of before and after claim hooks, and ensuring proper validation of claim recipients to maintain the integrity of the prize claiming process. |
| `HookManager.sol`       | The HookManager contract's core functionality is to allow each account to set and manage prize hooks that are executed when they win in PoolTogether V5.                                                                                                                                                                                                                | It implements a mapping to store user addresses and their associated hooks, providing functions to get and set hooks for each account.                                                                       | It enables customizable `prize-winning` behaviors for individual accounts, enhancing user engagement and flexibility within the `PoolTogether V5` ecosystem and Involves handling the configuration and execution of prize hooks by individual users, ensuring proper association and execution of hooks to maintain the integrity of prize-winning mechanisms.                                                                    |

## Systemic Risks, Centralization Risks, Technical Risks & Integration Risks

1.  **PrizeVault.sol**

    - **Systemic Risks:**

      1. `Yield Vault Failure`: The contract relies on an external yield vault to generate yield, and if the yield vault fails or is hacked, it could result in a loss of funds for the depositors.

      2. The contract relies on external contracts like `IERC4626`, `IERC20`, and `ILiquidationSource`. If any of these contracts fail or are compromised, it could affect the functioning of this contract.

      3. The contract implements a `yield fee` and a yield buffer. If the `yield fee` is set too high or the yield buffer is depleted, it could lead to a situation where depositors cannot withdraw their full deposit.

      4. `Market Volatility`: The value of the underlying asset deposited in the contract may fluctuate, resulting in a decrease in the value of the prizes awarded.

    - **Centralization Risks:**

      1. The contract has an `owner` who can set the yield fee percentage, yield fee recipient, and `liquidation pair`. If the owner becomes malicious or compromised, they could set these parameters in a way that benefits them at the expense of other users.

      2. The yield fee recipient has the ability to claim yield fee shares. If this address becomes compromised, the yield fees could be stolen.

      3. The `prize pool` is controlled by a single contract, which could lead to centralization of the prize distribution process and potential manipulation of the prize distribution, The `liquidation pair` is a single address, which could lead to centralization of the liquidation process and potential manipulation of the liquidation price, The `yield fee recipient` is a single address, which could lead to centralization of the fee collection and potential misuse of funds.

    - **Technical Risks:**

      1. `Yield Fee Calculation Error`: The yield fee calculation may contain errors, resulting in an incorrect fee being charged to the depositors.

      2. `Yield Buffer Depletion`: The yield buffer may be depleted, resulting in new deposits being prevented and the vault entering a lossy withdrawal state where depositors will incur the rounding errors on withdrawal.

      3. `Rounding Errors`: The contract uses a 'dust collection strategy' and a 'yield buffer' to handle rounding errors. However, if these strategies are not implemented correctly, it could lead to loss of funds.

    - **Integration Risks:**

      1. `Compatibility with Underlying Asset`: The contract assumes that the underlying asset has certain properties (e.g., it does not take a fee on deposit or withdraw). If these assumptions are not met, it could lead to unintended behavior.

      2. `Compatibility with Yield Vault`: The contract assumes that the yield vault does not incur any rounding errors. If this assumption is not met, it could lead to loss of funds.

      3. `Liquidation Pair`: The contract relies on a liquidation pair to liquidate yield for prize tokens. If the liquidation pair does not function as expected, it could affect the prize distribution.

2.  **PrizeVaultFactory.sol**

    - **Systemic Risks:**

      1. **Dependence on underlying yield vault:** The Prize Vault relies on the underlying yield vault for generating yield. If the yield vault experiences any issues or vulnerabilities, it could impact the Prize Vault's ability to generate yield and distribute prizes.
      2. **Counterparty risk:** The Prize Vault interacts with various third-party services, such as the underlying yield vault and the Prize Pool. If any of these third-party services experience issues or become insolvent, it could impact the Prize Vault's operations.

    - **Centralization Risks:**

      1. **Single point of failure:** The Prize Vault is managed by a single entity (`the owner`). If the owner becomes compromised or malicious, they could potentially misuse their control over the Prize Vault.
      2. **Limited oversight:** The Prize Vault is not subject to any external regulation or oversight. This means that the owner has significant autonomy in making decisions about the Prize Vault's operations.

    - **Technical Risks:**

      1. **Gas costs:** The Prize Vault's operations require gas to execute transactions on the blockchain. If gas costs increase significantly, it could make the Prize Vault less cost-effective to operate.
      2. **Scalability:** The Prize Vault is a relatively complex smart contract, and its performance could be impacted if the number of users or transactions increases significantly.
      3. **Interoperability:** The Prize Vault is designed to work with a specific underlying yield vault and Prize Pool. If the yield vault or Prize Pool changes or updates its protocols, it could require significant effort to update the Prize Vault to remain compatible.

    - **Integration Risks:**

      1. **Compatibility issues:** The Prize Vault is designed to work with a specific underlying yield vault and Prize Pool. If the yield vault or Prize Pool changes its protocols or interfaces, it could require significant effort to update the Prize Vault to remain compatible.
      2. **Data integrity:** The Prize Vault relies on data from the underlying yield vault and the Prize Pool. If there are any inaccuracies or inconsistencies in this data, it could impact the Prize Vault's calculations and operations.

3.  **TwabERC20.sol**

    - **Centralization Risks**:

      1. **Dependency Centralization**: The contract relies heavily on external dependencies, particularly from the OpenZeppelin library. Centralizing functionality in external dependencies can introduce central points of failure or control.
      2. **TwabController Dependency**: The contract tightly couples with the `TwabController` contract, which could introduce centralization risks if the `TwabController` becomes compromised or malicious.

    - **Technical Risks**:

      1. **SafeCast Usage**: The contract uses `SafeCast` from OpenZeppelin to safely cast `uint256` to `uint96`. While this is generally safe, incorrect usage of `SafeCast` or changes in the underlying data types could introduce technical risks.
      2. **ERC20Permit Usage**: The contract inherits from `ERC20Permit`, which implements the permit function allowing token approvals with signatures. Any vulnerabilities in the `ERC20Permit` implementation could pose technical risks to this contract.

    - **Integration Risks**:

      1. **Integration with TwabController**: The contract integrates tightly with the `TwabController` contract. Any changes or vulnerabilities in the `TwabController` contract could impact the functionality and security of this contract.
      2. **External Integration Risks**: The contract integrates with external ERC20 tokens and potentially other contracts in the ecosystem. Any changes or vulnerabilities in these external integrations could introduce risks to this contract.

4.  **Claimable.sol**

    - **Systemic Risks**:

      1. **Dependency Risks**: The contract imports external dependencies (`IClaimable.sol`, `PrizePool.sol`, `HookManager.sol`). Any vulnerabilities or changes in these dependencies could impact the security and functionality of this contract.

    - **Centralization Risks**:

      1.  **Dependency Centralization**: Similar to the previous contract, this contract relies on external dependencies. Heavy reliance on external dependencies can introduce central points of failure or control.
      2.  **Claimer Address**: The contract has a single `claimer` address, which introduces centralization risk if the claimer address becomes compromised or malicious.

    - **Technical Risks**:

      1. **Gas Limitation**: The contract defines a constant `HOOK_GAS`, which determines the gas limit for before and after claim hooks. Setting a fixed gas limit could introduce technical risks if the gas consumption of the hooks exceeds this limit.
      2. **Hook Implementation**: The contract uses hooks that execute before and after prize claim. Any vulnerabilities or incorrect implementation in these hooks could pose technical risks to the contract's functionality and security.

    - **Integration Risks**:

      1. **Integration with PrizePool**: The contract integrates tightly with the `PrizePool` contract. Any changes or vulnerabilities in the `PrizePool` contract could impact the functionality and security of this contract.
      2. **External Integration Risks**: Similar to the previous contract, this contract integrates with external contracts (`IClaimable.sol`, `PrizePool.sol`). Any changes or vulnerabilities in these external integrations could introduce risks to this contract.

5.  **HookManager.sol**

    - **Systemic Risks**:

      1.  **Dependency Risks**: The contract imports an interface `IVaultHooks.sol`. Any vulnerabilities or changes in this dependency could impact the security and functionality of this contract.

    - **Centralization Risks**:

      1.  **Storage Centralization**: The contract centrally stores hooks for each account in a mapping. Centralized storage of hooks introduces a risk of manipulation or unauthorized access.
      2.  **Control over Hooks**: Accounts have complete control over setting their own hooks, which could lead to centralization if certain accounts have disproportionate influence or control over the system.

    - **Technical Risks**:

      1.  **Access Control**: The contract doesn't have explicit access control mechanisms for setting hooks. Depending on the implementation of `IVaultHooks.sol`, there could be technical risks related to unauthorized access or manipulation of hooks.

    - **Integration Risks**:

      1.  **Integration with IVaultHooks**: The contract integrates with the `IVaultHooks` interface. Any changes or vulnerabilities in the implementation of `IVaultHooks` could impact the functionality and security of this contract.

## Suggestions

### What could they have done better?

1. If we look at the test scope and content of the project with a systematic checklist, we can see which parts are good and which areas have room for improvement As a result of my analysis, those marked in green are the ones that the project has fully achieved. The remaining areas are the development areas of the project in terms of testing ;

[![test-cases.jpg](https://i.postimg.cc/1zgD5wCt/test-cases.jpg)](https://postimg.cc/v1s40gdF)

Ref:https://xin-xia.github.io/publication/icse194.pdf

[![nabeel-1.jpg](https://i.postimg.cc/6qtBdLQW/nabeel-1.jpg)](https://postimg.cc/bDVXPnbW)

2. It is recommended to increase the test coverage to 100% so make sure that each and every line is battle tested

### What ideas can be incorporated?

1. **Emergency Withdraw**: An emergency withdraw function could be added to allow users to withdraw their assets in case of an emergency or if the yield vault becomes unsafe. This function could be only callable by the owner or through a governance vote.

2. **Yield Strategies**: The contract could be extended to support multiple yield strategies, allowing users to choose the strategy that best suits their risk tolerance and yield expectations.

3. **Staking**: A staking mechanism could be added to incentivize users to keep their assets in the vault for longer periods. This could involve rewarding users with additional prize tokens or a higher chance of winning a prize based on the length of time their assets have been staked.

4. **Auto-Compounding**: The contract could be designed to automatically reinvest the yield back into the vault, compounding the user's assets over time.

5. **Yield Insurance**: An insurance mechanism could be added to protect users against potential losses in the yield vault. This could involve charging an additional fee that is used to purchase insurance or build up an insurance fund.

6. **Dynamic Yield Fee**: The yield fee could be made dynamic, adjusting based on market conditions or the performance of the yield vault. This could help ensure that the yield fee remains competitive and that the yield buffer is sufficient to cover rounding errors.

7. **Yield Token**: The contract could be extended to mint a yield token that represents a user's share of the yield. This token could be traded or used to claim a share of the yield at a later time.

8. **Governance**: A governance mechanism could be added to allow users to vote on changes to the contract, such as the yield fee, yield buffer, or yield vault. This could help ensure that the contract is managed in a way that benefits all users.

### What’s unique?

The PrizeVault contract is a redesigned and refactored version of the previous Vault contract used in PoolTogether. It is designed to be fully compliant with the ERC4626 specification and interface cleanly with underlying yield vaults. The PrizeVault employs two strategies to cover rounding errors and ensure that depositors can withdraw every last wei of their initial deposit:

Dust collection strategy: Mints yield vault shares directly to reduce the severity of rounding errors during deposit and withdrawal.
Yield buffer: Ensures that there is always enough yield reserved to cover the rounding errors on deposits and withdrawals.
PoolTogether V5 brings several improvements, including:

Autonomous: There is no central entity controlling the protocol.
Permissionless: Anyone can add new assets and yield sources to the protocol.
Adaptive prize count: Adjusts the number of prize tiers based on how many prizes were claimed for the previous awarded draw.
When users deposit tokens into PoolTogether, they are depositing into an ERC4626 vault that tracks deposits using the Twab Controller. Vaults claim prizes on behalf of their users, which gives Vaults flexibility in how they want to handle prizes. The default PoolTogether Vault has a separate Claimer contract through which users claim prizes for others.

The PrizeVault contract is an implementation of a prize vault for the PoolTogether V5 protocol. It takes deposits of an asset, earns yield with the deposits through an underlying yield vault, and then contributes the yield to a prize pool as prize tokens. Depositors of the prize vault are then eligible to win prizes from the pool. The contract employs two strategies, the dust collection strategy and the yield buffer, to ensure that depositors can always withdraw their full deposit amount, even if the underlying yield source loses assets. The contract also includes functions for setting the yield fee percentage and yield fee recipient, as well as for claiming yield fee shares.

## Issues surfaced from Attack Ideas in [README](https://github.com/code-423n4/2024-03-pooltogether?tab=readme-ov-file#attack-ideas-where-to-look-for-bugs)

The following are recommended places to start looking for bugs/issues. This list is non-exhaustive, so make sure to follow your own instincts as well!

- Accounting Issues
- TwabERC20 ERC20 standard compliance
- PrizeVault ERC4626 standard compliance
- Reentrancy Exploits (deposits, withdrawals, prize hooks)
- Yield Source Integration Compatibility (ex. how is asset loss handled? what can break the integration?)
  - [Yearn V3](https://github.com/yearn/yearn-vaults-v3)
  - [Beefy](https://docs.beefy.finance/developer-documentation/other-beefy-contracts/beefywrapper-contract)
  - [sDAI](https://docs.spark.fi/defi-infrastructure/sdai-overview)
  - [Yield Daddy Aave V3 Wrapper](https://github.com/timeless-fi/yield-daddy/blob/main/src/aave-v3/AaveV3ERC4626.sol)
  - [Yield Daddy Lido Wrapper](https://github.com/timeless-fi/yield-daddy/blob/main/src/lido/StETHERC4626.sol)

It's also important that we don't repeat the same mistakes from the past! Check out these two previous issues that were found in the prior version of the vault:

- [Forceful deposit through `depositWithPermit`](https://gov.pooltogether.com/t/v5-vault-medium-risk-disclosure/3124)
- [Miscalculation of `totalAssets` in vaults using Aave V3](https://gov.pooltogether.com/t/v5-vault-collateralization-issue/3170)



### Time spent:
30 hours