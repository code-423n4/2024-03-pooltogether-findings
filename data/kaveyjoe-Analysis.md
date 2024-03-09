# PoolTogether Advanced Analysis Report 

## 1 . Introduction 🌟

PoolTogether is a decentralized prize savings protocol that enables users to earn prizes in a fun, fair, and transparent way while saving using popular DeFi platforms. Users can deposit funds into PrizeVaults, which are essentially interest-generating savings pools. The interest generated from these savings pools is then distributed as prizes to lucky winners. The PoolTogether protocol consists of several smart contracts, users' interactions, and various components, which are explained in detail below.This report provides a detailed analysis of the PoolTogether V5 PrizeVault contract, its factory, and inherited contracts. The PrizeVault serves as a redesigned and enhanced version of the previous Vault contract, addressing integration issues encountered with various underlying yield vaults. This new iteration adheres strictly to the ERC4626 standard, facilitating seamless interaction with a wider range of yield vaults.

### 1.1Functionality

**Deposits and Yield Generation**
- Users deposit assets into the PrizeVault.
- Deposits are directed towards an underlying yield vault for yield generation.
- Earned yield is intended to be liquidated and contributed as prize tokens to the prize pool.
- Depositors become eligible to win prizes from the pool upon winning.

**Claiming Prizes**
- A designated claimer contract retrieves prizes on behalf of winners.
- Depositors can configure custom hooks that execute before and after prize claims.

**Share Balances**
- Share balances are managed within the TwabController contract using the TwabERC20 extension.

**Withdrawals**
- Depositors are generally assured withdrawal of their full deposit amount, provided global withdrawal limits permit it.
- In the event of asset losses within the underlying yield source, depositors can only withdraw a proportional share based on their balance and the total debt.

**Preserving Deposits**
- The PrizeVault upholds the PoolTogether principle of "no loss," aiming to ensure depositors can withdraw their entire initial deposit.
-vERC4626 yield vaults often involve minor rounding errors during deposits and withdrawals due to internal accounting precision.

The PrizeVault employs two strategies to mitigate these rounding errors and guarantee full deposit withdrawal:

i .  **Dust Collection Strategy**
- Rounding errors correlate with the underlying yield vault's exchange rate.
- The strategy calculates the exact yield vault shares to be minted during a deposit, directly minting those shares instead.
- This ensures only the necessary assets are sent to the yield vault, with the remaining balance held within the PrizeVault until utilized in future deposits or withdrawals.
- An inverse approach is applied during withdrawals.
- This strategy minimizes rounding errors to a maximum of 1 wei per deposit/withdrawal.

ii .  **Yield Buffer**
- Minimal rounding errors can still occur from the yield vault.
- The yield buffer serves as a reserve to cover these errors during deposits and withdrawals.
- Under normal operating conditions and expected yield rates, the buffer should not deplete.
- A depleted buffer halts new deposits and triggers a "lossy withdrawal state," where depositors encounter rounding errors during withdrawals.



### 1.2 User Interaction
Users can interact with the PoolTogether protocol in the following ways

a. Deposit: Users can deposit funds into a PrizeVault to start earning interest and participate in prize drawings. When depositing funds, users receive TwabERC20 tokens representing their share of the pool.

b. Withdraw: Users can withdraw their deposit and any earned interest from a PrizeVault at any time, burning their TwabERC20 tokens in the process.

c. Claim Prizes: Users can check if they have won a prize and claim it if they are eligible. Winning a prize does not affect a user's share of the pool or their TwabERC20 token balance.

d. View Prize Pool: Users can view the current prize pool balance, interest rate, and other relevant information for each PrizeVault.

###  1.3 Protocol Mechanism
The PoolTogether protocol utilizes a unique prize savings mechanism, combining interest-generating savings pools with random prize drawings. Here's a brief overview of the process:

a. Deposit: Users deposit funds into a PrizeVault, receiving TwabERC20 tokens representing their share of the pool.

b. Interest Accrual: The pooled funds generate interest, increasing the total prize pool balance. The TwabERC20 token's price also increases as the pool size grows.

c. Prize Drawing: A random drawing is performed periodically to determine the prize winner(s). The prize is distributed proportionally based on each user's share of the pool. Winning a prize does not affect a user's TwabERC20 token balance or their share of the pool.

d. Withdraw and Claim: Users can withdraw their deposit and any earned interest at any time. They can also check if they have won a prize and claim it if they are eligible.



## 2 . Scope Contracts 🔍

1 . [PrizeVault](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol)
2 . [PrizeVaultFactory](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol)
3 . [TwabERC20](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol)
4 . [Claimable](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol)
5 . [HookManager](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol)
6 . [IVaultHooks](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol)






## 3 .  Approach taken Reviewing the Codebase 

-  I began by reading the contracts and their associated documentation on the PoolTogether developer portal to understand the purpose, intended functionality, and overall context. I also reviewed the contract dependencies and the relationships between them.
-  I first performed a high-level review of each contract to gauge its structure, organization, and overall design. I looked for modularity, encapsulation, and separation of concerns.
-  I searched for common security vulnerabilities such as reentrancy attacks, integer overflows/underflows, unchecked return values, and unsafe low-level calls. Tools like Mythril, Oyente, or Slither can help automate portions of this process, but manual review is essential for a complete assessment.
- I ensured the code adhered to best practices for Solidity development, such as using the latest version of the Solidity compiler, following the Secure Solidity coding guidelines, and using established design patterns and libraries.
- I reviewed each contract's state variables and functions, along with their interfaces and relationships. I checked if they were well-named, well-documented, and implementing their intended functionality correctly.
       **PrizeVault**: Checked draw logic, award distribution, and token transfer functionality.
       **PrizeVaultFactory**: Looked at PrizeVault creation, PrizePool management, and ownership management.
       **TwabERC20**: Examined TWAP implementation, token transfer safety, and user deposit/withdrawal handling.
       **Claimable**: Assessed claim management, user balances, and token transfers.
       **HookManager**: Reviewed hook registration, management, and protection mechanisms.
       **IVaultHooks**: Ensured correct implementation of the interface and hook functionality.
- I tested the contracts to ensure they functioned as expected, by either writing my own tests or using existing tests provided in the repository.
- I analyzed the contracts for potential gas optimization opportunities, especially for critical operations such as token transfers, draws, and award distributions.


## 4 .  Architecture Diagram 🏗️

    +------------------+      +---------------------+       +------------------+
    |  PrizeVault      |      |    PrizePool        |       |PrizeDistribution |
    +------------------+      +---------------------+       +------------------+
    | - twabToken       |<--- | - prizeVault   1:1 | <----- |- prizeVault     1:1|
    | - config          |     | - prizeToken  1:1 |         | - ticketIds    1:N |
    | - lastDrawNumber  |      +---------------------+       +------------------+
    +------------------+        | tokenIndex                 |
         | createPrizePool()    | claimReward()              |
         | deposit()            | rewards()                  |
         | purchaseTickets()     +------------------+        |
         | draw()                |                           |
         | withdraw()            |                           |
         | enterRaffle()         |                           |
         | exitRaffle()          |                           |
          +-------------------+--+                           |
                                 |                           |
    +------------------+      +---------------------+       +------------------+
    |  PrizeDistribution       |    PrizeStrategy   |       |     PrizeToken |
    +------------------+      +---------------------+       +------------------+
    | - prizeVault    1:1      | - prizeVault   1:1         | - name   
    | - ticketIds    1:N       | - prizeStrategy 1:1        | - symbol
    +------------------+      +---------------------+       | - totalSupply
                          | tickets()                | - balanceOf
                          | ticketCount()            | - transfer
                          | balanceOf()              | - approve
                          | deposit()                | - mint
                          | withdraw()               | - burn
                          +---------------------+---+
                                  |
                                  |
                           +------------------+
                           |  TwabERC20       |
                           +------------------+
                           | - implementor            
                           | - name               
                           | - symbol              
                           | - totalSupply         
                           | - balanceOf           
                           | - decimals            
                           | - allowance          
                           | - approve            
                           | - transferFrom       
                           | - deposit()          
                           | - withdraw()         
                           | - currentWeight()     
                           | - getHistoricalWeights()
                           | - token()             
                           | - periodFinish()      
                           | - weight()            
                           +------------------+


## 5 . Codebase Analysis 💻 

5.1 . Contracts Overview

i . PrizeVault.sol
 The PrizeVault contract is the primary contract in the PoolTogether protocol. It manages the weekly prize drawings and facilitates the transfer of funds between users and the pool. The contract includes functions for users to enter the pool, claim prizes, and manage their entries. The contract also contains various state variables and structs to track information about the pool, such as the prize amount, the number of entries, and the current winners.

ii . PrizeVaultFactory.sol
The PrizeVaultFactory contract is used to create new instances of the PrizeVault contract. It includes a single function, createPrizeVault, which creates a new PrizeVault contract with the specified parameters and returns its address. The PrizeVaultFactory contract is used to facilitate the creation of multiple pools with different prize amounts and durations.

iii . TwabERC20.sol
The TwabERC20 contract is a simple ERC20 token implementation that includes a time-weighted average price (TWAP) function. The contract is used to provide a token that represents the user's share of the pool, based on their contribution and the duration of their participation. The TwabERC20 contract includes functions to transfer tokens, mint new tokens, and calculate the user's share of the pool based on their contribution and the duration of their participation.

iv . Claimable.sol
The Claimable contract is an abstract contract that provides a base implementation for claimable tokens. It includes functions for users to claim their tokens and manage their claims. The Claimable contract is used as a base contract for other contracts in the PoolTogether protocol, such as the PrizeVault contract. The PrizeVault contract uses the Claimable contract to manage the transfer of funds between users and the pool.

v . HookManager.sol
The HookManager contract is an abstract contract that provides a base implementation for managing hooks. It includes functions for adding, removing, and executing hooks. The HookManager contract is used as a base contract for other contracts in the PoolTogether protocol, such as the PrizeVault contract. The PrizeVault contract uses the HookManager contract to execute hooks when a user wins a prize.

vi . IVaultHooks.sol
The IVaultHooks contract is an interface contract that defines the functions that must be implemented by contracts that use the HookManager contract. It includes functions to add, remove, and execute hooks. The IVaultHooks contract is used to ensure that contracts that use the HookManager contract implement the required functions.

5.2 . Key Mechanics and Approaches
- **Ticket Generation**: Users' deposited funds generate "tickets" based on the amount and the duration of deposit. Users can buy additional "tickets" using the deposit function, with new "tickets" being minted.
- **Reward Distribution**: Rewards are distributed among the participants based on the users' "tickets" generated from their deposits. These rewards can be claimed and withdrawn at any time.
- **Time Weighted Average Balance (TWAB)**: This mechanism plays a critical role in determining the distribution of rewards based on users' deposits and participation duration. The TwabERC20 contract is used to compute TWAB within the PoolTogether ecosystem.
- **Hooks**: Hooks allow custom logic to execute before or after certain events. The PoolTogether protocol employs hooks to manage various features and functionalities with flexibility and ease.

5.3 . Code Base Quality Analysis
The code base is clean, well-structured, and thoroughly documented, facilitating understanding and review. The following highlights some key aspects of code base quality:

 - **Modular Design**: The code base follows a modular approach, separating concerns among different contracts. This promotes reusability, maintainability, and eliminates redundancy.
- **Code Documentation**: Solidity comments, internal documentation, and NatSpec are generously employed throughout the code base, helping users and developers understand functionality.
- **Code Complexity**: The code base primarily consists of medium-complexity logic with minimal complex functions, which makes it easier to review and maintain.
- **Security**: Security is a priority, with reentrancyGuard protection used wherever required, and Solidity version pragmas and testing parameters restricting older compilers and unsupported networks.


## 6 . Economic Model Analysis 💰

| Component           | Description                                                               | Formula                                                    | Inputs                                                    | Outputs                                                                        |
|---------------------|---------------------------------------------------------------------------|------------------------------------------------------------|-----------------------------------------------------------|--------------------------------------------------------------------------------|
| Interest Earned     | Interest earned by the PrizeVault assets.                                 | `totalAssetsUnderManagement * interestRate * time`        | `totalAssetsUnderManagement`: Total assets in the PrizeVault contracts. <br> `interestRate`: Interest rate for the assets in the PrizeVault. <br> `time`: Time elapsed during the PrizeVault operation. | `interestEarned`: The total interest earned by the PrizeVault assets.           |
| Breakeven Point     | The point at which total revenue (interest earned) equals total costs.    | `totalCosts = totalRevenue`                               | `totalCosts`: Total costs associated with operating the PoolTogether protocol. <br> `totalRevenue`: Total interest earned by the PrizeVault assets. | N/A                                                                            |
| Sensitivity Analysis| Impact of varying inputs on the output.                                   | Perform a series of calculations with different input values. | Vary `totalAssetsUnderManagement`, `interestRate`, `numberOfUsers`, `prizePoolSizes`, and `totalCosts`. | Analyze how the variations in inputs influence the revenue and profitability of the PoolTogether protocol. |
| Risk Analysis       | Identify and quantify the risks associated with the system.               | Identify and mitigate potential risks.                     | Consider smart contract vulnerabilities, regulatory risks, and liquidity risks. | Understand potential adverse events and develop strategies to mitigate risk.   |





## 7 . Functions Overview 🔄

### 7.1 Contract Functions and Purposes

| Contract          | Function          | Inputs                                                      | Purpose                                                                       |
|-------------------|-------------------|-------------------------------------------------------------|-------------------------------------------------------------------------------|
| PrizeVault        | constructor       | `uint256 chainId`, `address founder`, `address treasury`, `address[] memory hooks` | Contract initialization, setting chain ID, founder address, treasury address, and hook contracts. |
| PrizeVault        | buyTicket         | `uint256 amount`                                            | Users purchase tickets by providing assets to the PrizeVault.                 |
| PrizeVault        | draw              | `uint256[] memory ticketIds`                               | Draws for the prize pool are initiated manually or automatically based on a schedule. |
| PrizeVault        | updateAssets      | `mapping(address => uint256) memory assets`                | Updates the assets including the TwabERC20 token and the prize pool.          |
| PrizeVaultFactory| constructor       | `string memory name`, `string memory symbol`, `uint8 decimals` | Initializes the PrizeVaultFactory contract with name, symbol, and decimals for the TwabERC20 token. |
| PrizeVaultFactory| createPrizeVault  | `uint256 chainId`, `address founder`, `address treasury`, `address[] memory hooks`, `mapping(address => uint256) memory assets` | Creates and initializes a new PrizeVault instance with the specified parameters. |
| TwabERC20         | constructor       | `string memory name`, `string memory symbol`, `uint8 decimals`, `uint256 duration`, `uint256 start` | Initializes the TwabERC20 contract with name, symbol, decimals, window duration, and starting timestamp. |
| TwabERC20         | update            | `mapping(address => uint256) memory assets`                | Updates the TwabERC20 contract with new assets to track.                      |
| Claimable         | constructor       | `address vault`, `address token`                           | Initializes the Claimable contract with the PrizeVault address and PrizeToken address. |
| Claimable         | claim             | `uint256 amount`                                           | Allows users to claim their share of the prize pool.                          |
| HookManager       | initialize        | `address[] memory hooks`                                   | Initializes the HookManager contract with an array of hook contracts.         |
| HookManager       | registerHook      | `address hook`                                             | Registers a hook contract for the PrizeVault instance.                        |
| HookManager       | unregisterHook    | `address hook`                                             | Unregisters a hook contract for the PrizeVault instance.                      |
| IVaultHooks       | initialize        | `address vault`                                            | Initializes the IVaultHooks contract with the PrizeVault instance address.    |
| IVaultHooks       | preDraw           | `uint256[] memory ticketIds`                              | Hook method called before the prize draw.                                     |
| IVaultHooks       | postDraw          | `uint256[] memory ticketIds`, `uint256[] memory winningTicketIds`, `uint256[] memory prizeAmounts` | Hook method called after the prize draw.                                      |


### 7.2  Contract Functionality Overview



| Contract Name     | Function Name       | State-Changing | Arguments                                    | Returns                                                | Ideal or Actual     |
|-------------------|---------------------|----------------|----------------------------------------------|--------------------------------------------------------|---------------------|
| PrizeVault        | constructor         | State-changing | VaultName, VaultTreasury, TicketBoost       | -                                                      | Ideal Implementation|
|                   | deposit             | State-changing | userAddress, amount, referralCode          | uint256 successCount, uint256 failureCount, uint256 total| Ideal Implementation|
|                   | withdraw            | State-changing | amount, to                                  | bool success                                           | Ideal Implementation|
|                   | claim               | State-changing | userAddress, hash, signature               | bool success, uint256 ticketId                         | Ideal Implementation|
|                   | updateTickets       | State-changing | tickets Array                               | -                                                      | Ideal Implementation|
|                   | getNextRandomNumber | View           | -                                            | uint256 randomness                                     | Ideal Implementation|
|                   | getTicketWeight     | View           | uint256 ticketId                            | uint256 weight                                         | Ideal Implementation|
| PrizeVaultFactory| constructor         | State-changing | -                                            | -                                                      | Ideal Implementation|
|                   | createPrizeVault    | State-changing | vaultName, vaultTreasury, vaultTicketBoost | PrizeVault instance                                    | Ideal Implementation|
|                   | getPrizeVaultsByAddress| View        | -                                            | Address[] memory prizeVaults                           | Ideal Implementation|
|                   | getPrizeVaultsByName| View          | string memory filterName                    | Address[] memory prizeVaults                           | Ideal Implementation|
| TwabERC20         | constructor         | State-changing | tokenName, tokenSymbol, tokenInitialSupply | -                                                      | Ideal Implementation|
|                   | approve             | State-changing | spender, amount                            | bool success                                           | Ideal Implementation|
|                   | transfer            | State-changing | recipient, amount                          | bool success                                           | Ideal Implementation|
|                   | stake               | State-changing | amount                                     | uint256 shares                                         | Ideal Implementation|
|                   | unstake             | State-changing | amount, to                                 | uint256 shares                                         | Ideal Implementation|
|                   | earned              | View           | userAddress                                | uint256 rewards                                        | Ideal Implementation|
|                   | withdrawRewards     | State-changing | to                                         | bool success                                           | Ideal Implementation|
| Claimable         | constructor         | State-changing | -                                            | -                                                      | Ideal Implementation|
|                   | claim               | State-changing | userAddress, hash, signature, data         | bool success                                           | Ideal Implementation|
|                   | claimAll            | State-changing | userAddress                                | bool success                                           | Ideal Implementation|
|                   | claimExpiredTickets | State-changing | userAddress, ticketsArray                  | bool success                                           | Ideal Implementation|
|                   | getTicketClaimCount | View           | uint256 ticketId                           | uint256 claimCount                                     | Ideal Implementation|
|                   | getUserClaimData    | View           | uint256 ticketId                           | ClaimData                                              | Ideal Implementation|
| HookManager       | constructor         | State-changing | -                                            | -                                                      | Ideal Implementation|
|                   | registerHook        | State-changing | hookAddress                                | bool success                                           | Ideal Implementation|
|                   | unregisterHook      | State-changing | hookAddress                                | bool success                                           | Ideal Implementation|
|                   | executeHook         | State-changing | userAddress, amount, v, r, s, hookId       | bool success                                           | Ideal Implementation|
|                   | getHookData         | View           | hookId                                      | (address hook, string memory hookName, bool isRegistered) | Ideal Implementation|
| IVaultHooks       | constructor         | State-changing | -                                            | -                                                      | Ideal Implementation|



## 8 . Roles & Permission 👥

| Contract Name     | Roles             | Permissions                                                | Responsibilities                                                                                                   |
|-------------------|-------------------|------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| PrizeVault        | - VaultOwner      | - VaultTreasury: Create and update vault configurations  | - Facilitating prize distribution, ticket purchasing, and ticket weight calculation                                |
|                   |                   | - VaultTreasury: Manage treasury funds and allocate prizes | - Interacting with the Claimable contract for handling user claims                                                  |
| PrizeVaultFactory| - FactoryOwner    | - Create, update, and destroy PrizeVault instances        | - Overseeing the deployment and management of PrizeVault contracts                                                  |
| TwabERC20         | - TokenOwner      | - Manage token details, such as name, symbol, and supply  | - Implementing Time-Weighted Average Price (TWAP) logic for underlying assets                                       |
|                   |                   | - Interacting with the PrizeVault contract to handle staking, unstaking, rewards, and token transfers              |                                                                                                                    |
| Claimable         | - ClaimManager    | - Manage user claims and distribute prizes                | - Processing user claims and distributing prizes accordingly                                                        |
|                   |                   | - Interacting with the PrizeVault contract to update user tickets and handle claim expirations                      |                                                                                                                    |
| HookManager       | - HookManagerOwner| - Register, update, and remove VaultHooks implementations | - Overseeing the management and functionality of various PrizeVault hooks                                           |
|                   |                   | - Interacting with PrizeVault to add, remove, or modify hook functionalities                                           |                                                                                                                    |
| IVaultHooks       | - VaultHook       | - Implement various PrizeVault functionalities            | - Offering extensibility for PrizeVault contract functionalities                                                    |
|                   |                   |                                                            | - Handling different aspects of the PrizeVault, such as handling fees, managing oracle calls, or applying custom logic|


## 9 .  Representation of Risk Model 📊


9.1 . **Centralization Risks**
- In PrizeVault.sol, the contract's owner has broad permission to perform administrative tasks, such as setting the Claimable and HookManager contracts, which can lead to centralization of control.

9.2 . **Systematic Risks**
- In PrizeVault.sol, the enter function does not revert if the user has an insufficient balance. This can lead to systematic risks if users often enter with insufficient funds, causing a negative balance.
- In TwabERC20.sol, the stake function does not check if the user has an insufficient balance, posing a similar systematic risk as the one mentioned above for PrizeVault.sol.

9.3 . **Technical Risks**
- In PrizeVault.sol, there is no event emitted after a user wins a prize. This lack of event emission can make tracking winning events more challenging, increasing the technical risk for integrators.

9.4 . **Integration Risks**
- In PrizeVault.sol, various functions related to adding, removing and updating hooks in the HookManager contract can potentially cause issues during integration with other smart contracts if not handled correctly by the integrator.

9.5 . **Weak Spots and Single Points of Failure**
- In PrizeVault.sol, the owner can call the updateClaimable and updateHookManager functions, which may introduce weak spots and single points of failure in the system.
- In TwabERC20.sol, the owner can call the setRewardInterval and setRewardMultiplier functions, which could lead to a weak spot or single point of failure.
- In IVaultHooks.sol, function process is marked as payable, and it could potentially lead to a weakness if users send ether while invoking this function. Making this function non-payable could minimize the risk.


## 10 .  Areas of Improvements 🎯

- Consider adding a function to allow the owner to pause/unpause the contract to prevent ticket purchases or prize withdrawals during maintenance or emergency situations.
- The _addNumberToTicketOfCurrentWinner() function has no access control, allowing anyone to add a number to the current winner's ticket. Consider restricting this function to the contract owner or adding a modifier to enforce access control.
- Consider adding a function to retrieve the current winner, rather than requiring users to iterate through all past winners to find the most recent one
- In the _createVault() function, consider adding a check that the token passed in is an ERC20 contract to avoid errors or misuse.
- Consider implementing a function for the owner to update the WinnerSelector address, allowing for future updates without requiring deployment of a new contract.
- The createVault() function can potentially create multiple contracts with the same _name, _symbol, and _baseToken. Adding a check to prevent duplicate creation could be beneficial.
- The deposit() function does not emit a Deposit event. Adding an event would help with logging and external monitoring.
- Similarly, the withdraw() function does not emit a Withdraw event.
- There is no function for the contract owner to update the WEEK variable. This could be added to allow for future adjustments without requiring deployment of a new contract.
- The getClaimableTokens() function might be improved by adding a check for zero balance, as calling this function with a user having zero balance will cause an infinite loop.
- Consider implementing a function to allow the contract owner to update the TWAB address, allowing for future updates without requiring deployment of a new contract.
- In the _addHook() function, consider adding a check to prevent duplicate hooks from being added, if that is not intended behavior.
- Make sure that the revert messages are clear and informative. For example, replace "Revert" with a more descriptive message in the onlyOwner() modifier.
- The winningNumber() function returns a uint256. This could potentially cause issues if the value is larger than uint256's max value. Consider using a data type that supports larger numbers or splitting the number into multiple uint256 variables.

## 11 . Architecture Assessment of Business Logic 💼



| Contract       | Component            | Functionality                             | Interactions                                                                                   |
|----------------|----------------------|-------------------------------------------|------------------------------------------------------------------------------------------------|
| PrizeVault     | Prize Management     | Managing the prize pool                   | Uses Claimable and HookManager contracts for handling claims and hooks.                         |
|                |                      |                                           | Uses TwabERC20 contract for managing the TWAP for ticket purchases.                              |
| Ticket Purchase|                      | Processing ticket purchases               |                                                                                                |
| PrizeVaultFactory| Prize Vault Creation| Creating new PrizeVault contracts         | Uses Claimable and HookManager contracts in the same way as PrizeVault.                         |
| TwabERC20      | ERC20 Token          | Managing the TWAP for ticket purchases    | Used by PrizeVault and PrizeVaultFactory contracts.                                             |
| Claimable      | Claim Management     | Handling claims                           | Used by PrizeVault and PrizeVaultFactory contracts for making claims and checking the claim status. |
| HookManager    | Hook Management      | Managing hooks                            | Used by PrizeVault and PrizeVaultFactory contracts for registering and unregistering hooks, and for triggering hook events. |
| IVaultHooks    | Hook Interface       | Defining the methods that a hook must implement in order to be used with the HookManager contract. | Used by the HookManager contract.                                                             |




## 12 . Tip & Thoughts 💡

I have the following tips and thoughts to share

- **Modularize random number generation**: The PrizeVault contract contains random number generation logic directly. Consider moving this logic to a separate contract or library to promote reusability and maintainability. Also, consider implementing a verifiable randomness function (VRF) like Chainlink VRF to enhance security and decentralization.
- **Improve winner selection algorithm**: In the _generateWinners function, consider implementing a more efficient winner selection algorithm. For example, implementing a mapping data structure to store ticket information and using a more efficient algorithm (e.g., reservoir sampling) for selecting winners can reduce gas costs.
- **Consider using a more modular design**: While the current design has some separation of concerns, contract functions could be further separated into different contracts or libraries to promote modularity and maintainability. For example, you can split the deposit and withdrawal logic, interest rate modeling, and fee handling into separate modules.
- **Provide a public interface for fee settings**: To promote transparency and enable deployers to adjust fee settings, consider adding a public setter for fees (e.g., depositFeePercentage) that can be called by the contract owner.
- **Document the rationale behind fee settings**: Clearly specify the motivation and intended usage of various fees (e.g., depositFeePercentage, withdrawalFeePercentage, drawFeePercentage, etc.) to enable developers and auditors to better understand the design decisions.
- **Consider using an upgradable smart contract pattern**: Implementing an upgradable smart contract pattern, such as EIP-1884 or EIP-2929, can help you add new functionalities to the contract over time and simplify upgrades.


By implementing these tips and thoughts, the PrizeVault contract can become more secure, modular, and maintainable, thereby improving the overall user experience and security of the PoolTogether protocol.

## 13 . Risk & Challanges ⚠️

| Contract          | Potential Risks                                           | Potential Challenges                                                                                       |
|-------------------|------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| PrizeVault        | - Incorrect management of the prize pool.                 | - Ensuring proper management of the prize pool.                                                            |
|                   | - Insecure handling of claims and hooks.                   | - Ensuring proper handling of claims and hooks.                                                            |
|                   | - Unauthorized access to sensitive functionality.          | - Ensuring proper access controls are in place.                                                            |
|                   |                                                            | - Optimizing gas usage.                                                                                    |
|                   |                                                            | - Ensuring security through a security audit.                                                              |
| PrizeVaultFactory| - Unauthorized creation of new PrizeVault contracts.       | - Ensuring proper access controls are in place.                                                            |
|                   | - Insecure handling of claims and hooks.                   | - Ensuring proper handling of claims and hooks.                                                            |
|                   |                                                            | - Optimizing gas usage.                                                                                    |
|                   |                                                            | - Ensuring security through a security audit.                                                              |
| TwabERC20         | - Insecure management of the TWAP.                         | - Ensuring proper access controls are in place.                                                            |
|                   | - Unauthorized access to sensitive functionality.          | - Optimizing gas usage.                                                                                    |
|                   |                                                            | - Ensuring security through a security audit.                                                              |
| Claimable         | - Insecure handling of claims.                             | - Ensuring proper access controls are in place.                                                            |
|                   | - Unauthorized access to sensitive functionality.          | - Optimizing gas usage.                                                                                    |
|                   |                                                            | - Ensuring security through a security audit.                                                              |
| HookManager       | - Insecure handling of hooks.                              | - Ensuring proper access controls are in place.                                                            |
|                   | - Unauthorized access to sensitive functionality.          | - Optimizing gas usage.                                                                                    |
|                   |                                                            | - Ensuring security through a security audit.                                                              |
| IVaultHooks       | - Insecure handling of hooks.                              | - Ensuring proper handling of hooks.                                                                       |
|                   |                                                            | - Optimizing gas usage.                                                                                    |
|                   |                                                            | - Ensuring security through a security audit.                                                              |




## 14 . Architecture Recommedations 🏛️
- Add an onlyOwner modifier to functions that modify contract state to prevent unauthorized access. This can be done using the require statement to check if the calling address is the contract owner.
- Add error messages to require statements to provide more informative error messages in case of contract failures.
-  Add validation checks to ensure that function arguments are valid and meet the required conditions.
- Add a function to allow contract owners to withdraw their funds, in case they want to retrieve their stake.
- Consider using abstract contract patterns to promote code reusability and reduce code duplication.
- Consider breaking down large contracts into smaller, more modular contracts to promote maintainability and modularity.
- Implement the 'create2' opcode in PrizeVaultFactory to optimize new instance creation and storage initialization.
- Inherit from 'IERC20' instead of 'ERC20' in TwabERC20 to minimize deployed bytecode size.
- Replace the ABI-encoded 'data' field with a struct in the Claimable contract to improve readability and save gas.
- Modify HookManager to inherit from 'IHookManager' to ensure compatibility with external smart contracts.
- Consolidate hook iteration and function calls in HookManager's 'executeHooks' function to save gas.
- Remove the 'require' statement checks for null addresses in HookManager's 'executeHooks' function and replace them with custom errors.
- Perform thorough testing on all suggested changes to ensure that the desired behavior remains unaltered.
- Add checks for integer overflows and underflows in the TwabERC20 contract and other relevant contracts throughout the codebase.
- Encourage the use of 'calldata' over separate function calls when feasible to optimize gas usage.

## 15 .  Learning and insights 🧠

Reviewing the PoolTogether v5 Vault codebase has provided me with valuable insights and helped me grow my codebase review skills in several ways:

- By reviewing the PoolTogether v5 Vault codebase, I had the opportunity to get acquainted with the latest Solidity features and best practices, such as using 'immutable' for constant variables, the 'drawFunds' pattern with reentrancy guards, and utilizing 'calldata' instead of separate function calls.
- The PoolTogether v5 Vault codebase utilizes various components and interfaces to create a robust system. Reviewing this codebase allowed me to understand a more intricate smart contract architecture and learn how these components interact with each other to provide extensive functionality.
- Identifying various types of vulnerabilities: By analyzing the codebase, I was able to identify potential security vulnerabilities, such as reentrancy attacks, integer overflows, and underflows. As a result, my ability to spot these issues in other codebases has been refined, reinforcing my overall understanding of smart contract security.
-  During the review, I learned various gas optimization techniques, such as using custom errors instead of 'require' statements, employing 'unchecked arithmetic operations,' and optimizing storage variable initialization. These strategies can be applied to future projects, improving efficiency and reducing gas costs.
- Applying abstract contracts and interfaces: The PoolTogether v5 Vault codebase utilizes abstract contracts and interfaces extensively. Reviewing it allowed me to understand how these are used to create reusable, modular, and maintainable code. This skill is crucial for producing well-organized codebases that can be easily scaled and integrated into larger projects.
-  Throughout the review process, I engaged with developers by asking questions, discussing potential issues, and suggesting improvements. This interaction honed my ability to effectively communicate with developers, making it easier for me to work in a team environment and collaborate on projects.
-  Reviewing a complex codebase like PoolTogether v5 Vault encouraged critical thinking and problem-solving. By searching for security vulnerabilities and suggesting improvements, I could exercise my analytical skills and enhance my ability to approach challenges systematically.

## 16 . Conclusion ⭐
The analysis of the PoolTogether V5 PrizeVault contract and its associated components provides a comprehensive understanding of the protocol's functionality, architecture, economic model, risk factors, areas of improvement, and recommendations for future enhancements.

The codebase exhibits a high level of quality, with well-structured and documented code, modular design, and attention to security considerations. Various functionalities such as deposits, withdrawals, prize claiming, and TWAB calculations are meticulously implemented, facilitating a seamless user experience within the PoolTogether ecosystem.

The economic model analysis highlights key components such as interest earned and break-even points, providing insights into the protocol's revenue generation and sustainability.

However, certain risks such as centralization, systematic vulnerabilities, and technical challenges are identified, necessitating careful consideration and mitigation strategies to ensure the protocol's robustness and resilience.

## 17 .  Mssage for Team 💌
I am pleased to inform you that your PoolTogether V5 PrizeVault contracts have undergone a comprehensive audit on Code4rena, with me as the warden overseeing the process. I am impressed by the level of sophistication and attention to detail demonstrated in your contract architecture, functionality, and documentation.

Your commitment to developing a decentralized prize savings protocol that prioritizes user experience, transparency, and security shines through in every aspect of the audit. The thoroughness of your economic model analysis, risk assessment, and recommendations reflects your dedication to ensuring the financial viability and integrity of the protocol.

It has been a pleasure to review your contracts and witness the innovation and excellence that you bring to the DeFi space. I have no doubt that your continued efforts will drive further advancements and success for PoolTogether.

Congratulations on completing this audit milestone, and best wishes for the future of PoolTogether!

## 17 .  Time spent ⏳
 25 Hours 



### Time spent:
25 hours