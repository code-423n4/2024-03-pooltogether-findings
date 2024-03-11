## Conceptual Overview

At its essence, the PoolTogether V5 system is built to work like a savings account with a twist; when you deposit your tokens, they're not just sitting idle. They get pooled together and put to work, earning interest. This interest doesn't go directly into your pocket, though. Instead, it's thrown into a collective pot which becomes a prize given out periodically. 

Diving into the codebase and its architecture reveals the meticulous implementation of this idea. Users start by interacting with what's called a 'Prize Vault,' a smart contract where their tokens are stored. But here's where it gets interesting—their funds aren't stagnant. They're put into another type of contract, known as an ERC-4626 vault, which is all about making those tokens work hard and generate yield (think interest or profit).

Now, you don't get this yield back as you would with a traditional savings account. It's converted into what the system calls 'prize tokens' through a process baked into the smart contracts. These prize tokens are essentially your lottery tickets—the more you have (based on how much and how long you save), the better your chances of winning the prize pool made up of the collective yield.

The protocol is built to be autonomous; it's self-running and doesn't need anyone pulling the strings for it to work. That's a big deal in the blockchain world—it means that once it's set up, it keeps going, come what may. The security here is in the code's immutability; it's tamper-proof.

One of the smartest bits is the Time-Weighted Average Balance feature. It's a way to calculate each user's chance of winning based on how much they've put in and for how long, ensuring the system is fair.

So, think of PoolTogether V5 as a fair, autonomous, and interest-driven system, designed with precision to generate and distribute yields in a way that's both engaging and equitable for users, providing a possibility of winning based on one's saving habits without risking the initial deposit.

[![conceptual.png](https://i.postimg.cc/pdBDfC6D/conceptual.png)](https://postimg.cc/Ff7dmymR)

## Technical Details (Architecture)

The PoolTogether V5 project manifests as a DeFi savings game articulated via several interconnected smart contracts and libraries, each executing distinct yet collaborative functionalities within the Ethereum ecosystem.

The cornerstone of the architecture is the **PrizeVault** contract, which extends the ERC20 standard to offer a tokenized representation of a user's deposit. It interfaces with a Yield Vault, a separate contract adhering to the ERC4626 standard, which is tasked with yield generation. The PrizeVault overrides typical ERC20 behaviors such as `transfer` and `balanceOf` to utilize the **TwabController**—a module that accounts for Time-Weighted Average Balances, ensuring that prizes are fairly distributed based on the duration and amount of a user's deposit.

A novel aspect of the PrizeVault is its `deposit` and `withdraw` functions, which not only adjust user balances but also interact with the Yield Vault to manage asset yield generation and prize token liquidation. This is further enhanced by an internal **liquidation mechanism**, which, upon the triggering by the **Liquidation Pair**, converts the yield into prize tokens via a process controlled by the **PrizePool** contract.

The PrizePool contract itself orchestrates the prize distribution logic. It operates autonomously, leveraging the blockchain's inherent immutability and security to manage prize draws without external intervention. The draw mechanism is predicated on randomness supplied externally, for which the **Draw Auction** strategy is pivotal, employing a Parabolic Fractional Dutch Auction to incentivize the delivery of random numbers from off-chain sources in a timely and cost-efficient manner.

Additionally, the **Claimable** extension allows for prize claims to be executed by an authorized Claimer contract. It can invoke user-defined hooks before and after the prize claim, as specified by the **HookManager** contract. This contract allows users to set callbacks (hooks) that can perform additional logic when a prize is won, a feature implemented in the `Claimable` contract's `claimPrize` function.

The protocol's economics are delicately balanced by the **Yield Fee** structure embedded within the PrizeVault. A nominal fee is levied on the yield generated, which is then redistributed to a specified Yield Fee Recipient, an incentive alignment mechanism ensuring protocol sustainability.

The **PrizeVaultFactory** facilitates the deployment of new PrizeVault instances, standardizing the creation process and ensuring consistency across deployments. It keeps track of all PrizeVaults it spawns, establishing a registry that can be referenced for verification or administrative purposes.

[![technicals.png](https://i.postimg.cc/K8fWzPDH/technicals.png)](https://postimg.cc/Mnc53j47)


## Asset-to-Share Mechanism

In the PoolTogether V5 system, the mechanics of asset-to-share conversion and the determination of liquidatable yield balance are grounded in specific financial formulas, as implemented in the `PrizeVault` contract and related to the ERC4626 standard. These calculations play pivotal roles in managing user deposits, yield generation, and prize distribution.

### Asset-to-Share Conversion:

The conversion functions `convertToShares(uint256 _assets)` and `convertToAssets(uint256 _shares)` embody the financial logic for translating between the deposited assets and the vault shares. The conversion is initially designed to follow a 1:1 ratio, simplifying the understanding that one deposited asset equals one share of the vault. However, this relationship may adjust based on the total yield accrued and the operational reserves maintained by the vault.

- **convertToShares**: Given the `totalDebt` (total amount owed to share holders, equivalent to total shares issued) and `totalAssets` (the sum of assets in the Yield Vault and any additional reserves), if `totalAssets` exceeds `totalDebt`, shares are converted directly at a 1:1 ratio. In a scenario where `totalAssets` is less than `totalDebt` (indicating a loss or excessive withdrawal fees), the conversion rate adjusts to ensure each share reflects a proportional amount of the remaining assets, using the formula: `shares = assets * totalDebt / totalAssets`.

- **convertToAssets**: This operation inversely converts shares back into assets, utilizing the same proportional logic to ensure fairness in distribution, especially in loss scenarios: `assets = shares * totalAssets / totalDebt`.

These formulas ensure that the process of depositing and withdrawing maintains the vault's integrity by accounting for yield fluctuations and operational reserves.

### Liquidatable Yield Calculation:

The `liquidatableBalanceOf(address _tokenOut)` function calculates how much of a specific asset or share can be liquidated from the yield generated, factoring in the operational reserves (yield buffer). This calculation starts by determining the total available yield, which is the excess of `totalAssets` over `totalDebt`, and then subtracts the yield buffer to safeguard against rounding errors and minor losses.

- **Total Available Yield**: Calculated as `totalYieldBalance = totalAssets - totalDebt`. This represents the total yield generated from the Yield Vault's operations.

- **Liquidatable Balance**: The actual amount available for liquidation (`availableYieldBalance`) is then derived by further subtracting the `yieldBuffer` from the `totalYieldBalance`. If the total yield exceeds the buffer, the liquidatable balance is the result of this subtraction. If not, the liquidatable balance is set to zero to protect the principal deposits.

The formula ensures sustainable prize distribution by maintaining a balance between the generated yield available for prizes and the reserves needed to cover potential operational inefficiencies. This delicate balance is crucial for the long-term viability of the no-loss lottery model, ensuring users are rewarded without compromising the principal deposits.

## Time-Weighted Average Balance (TWAB)

The Time-Weighted Average Balance (TWAB) feature is a pivotal component of PoolTogether V5, offering a unique and fair mechanism to determine prize eligibility based on the duration and amount of a user's deposit, rather than simply the instantaneous balance. This method calculates a depositor's average balance over time, ensuring that prizes are distributed more equitably among participants.

### Mathematical Foundation:

The TWAB calculation employs a method akin to the Exponential Moving Average (EMA) in finance, providing a rolling average balance that weights recent deposits more significantly than older ones. The key mathematical formula for TWAB involves recording "observations" at each deposit or withdrawal event, which consist of a timestamp, the current balance after the event, and a cumulative sum that incorporates the time-weighted balance up to that point.

An observation's cumulative balance is calculated using the formula:

$$\text{cumulativeBalance}_{\text{new}} = \text{cumulativeBalance}_{\text{previous}} + (\text{balance}_{\text{previous}} \times (\text{timestamp}_{\text{new}} - \text{timestamp}_{\text{previous}}))$$

This formula ensures that the contribution of each balance to the cumulative total is proportional to the time it was held, thus providing a time-weighted average when queried over any period.

### Implementation in Codebase:

In the codebase, the `TwabERC20.sol` contract interfaces with the `TwabController` to manage the TWABs for each depositor. When a user deposits or withdraws, the `TwabController` updates the user's balance and records a new observation. The smart contract calculates the new cumulative balance using the aforementioned formula, storing this alongside the new balance and timestamp as an observation.

The process to query a user's TWAB between two timestamps leverages the observations to interpolate (or directly use) the cumulative balances at those times, subtracting the earlier cumulative balance from the later one and dividing by the time elapsed. This effectively provides the average balance the user held within the specified period, implemented through the `getAverageBalanceBetween` function in the `TwabController`.

The significance of TWAB lies in its ability to democratize the chance of winning based on both the amount deposited and the length of time it is held, rather than favoring large, short-term deposits. This mechanism encourages longer-term participation in the PoolTogether protocol, aligning users' interests with the overall health and sustainability of the pool.

By integrating complex financial formulas with smart contract technology, PoolTogether V5's TWAB feature innovatively combines the principles of fairness, incentive alignment, and sustainability into its prize distribution mechanism.

[![TWAB.png](https://i.postimg.cc/rpTP8zG7/TWAB.png)](https://postimg.cc/SnZrdSvG)


##  Random Number Generation (RNG) Mechanism

The Random Number Generation (RNG) mechanism for prize allocation in PoolTogether leverages cryptographic methods to ensure fairness and transparency in distributing prizes. The use of Chainlink's Verifiable Random Function (VRF) serves as the cornerstone for generating unpredictable and tamper-proof random numbers. The mathematical and logical foundations of this process involve several key steps, integrating cryptographic security with probabilistic fairness.

The Chainlink VRF generates a random number $(R)$ through a process that combines the requesting smart contract's address, the user-supplied seed, and a nonce with the oracle's private key. The result is a random output $(R)$ and a proof $(P)$ that the number was generated according to the protocol and without external influence. The RNG process can be abstracted into the function
 $$\text{cumulativeBalance}_{\text{new}} = \text{cumulativeBalance}_{\text{previous}} + (\text{balance}_{\text{previous}} \times (\text{timestamp}_{\text{new}} - \text{timestamp}_{\text{previous}}))$$.

Upon generation, the smart contract verifies the validity of the random number by checking the cryptographic proof against the oracle's public key, ensuring the number was indeed produced by the oracle without tampering. This verification process can be mathematically represented as $\text{Verify}(R, P, \text{publicKey}) \rightarrow \text{true/false}$.

The integration of the RNG into prize allocation utilizes the TWAB (Time-Weighted Average Balance) of participants to determine winners fairly. Each participant's chance of winning is proportional to their TWAB, ensuring that participants with higher and longer stakes have better odds. Given a participant's TWAB $(T)$ and the total TWAB across all participants $T_{Total}$, the chance of winning for an individual can be conceptualized as $\frac{T}{T_{total}}$.

The allocation of prizes based on the random number $(R)$ is achieved by mapping \(R\) onto the cumulative distribution of participants' TWABs. This mapping involves calculating the fractional position $(F)$ of $(R)$ within the total TWAB range, defined as $F = \frac{R \pmod{T_{total}}}{T_{total}}$. The protocol then iterates through the ordered list of participants' TWABs until it finds the corresponding fraction $(F)$, thus identifying the winner.

This RNG mechanism, underpinned by cryptographic security and probabilistic fairness, ensures that the distribution of prizes in PoolTogether is both unpredictable and equitable, directly rewarding the magnitude and duration of participants' contributions to the pool.

[![RNG.png](https://i.postimg.cc/kg0Y47VB/RNG.png)](https://postimg.cc/zykF2ZFr)



## Other Functionalities of this Project

Diving deeper with a focus on the underlying logic and mathematical aspects, the **PrizeVaultFactory**, **Hook Management**, and **Prize Claiming** functionalities within the PoolTogether V5 project embody sophisticated mechanisms that facilitate its decentralized finance operations. 

### Vault Factories
The **PrizeVaultFactory** employs a CREATE2 opcode for deterministic deployment of PrizeVault contracts. This approach enables predictable contract addresses, enhancing user trust and interaction predictability within the ecosystem. The `deployVault` function intricately weaves together the parameters necessary for vault creation, setting a foundation for yield generation through ERC4626-compliant yield vaults. The initialization of a `YIELD_BUFFER` during vault deployment is a preemptive measure to counteract the minuscule but inevitable rounding discrepancies inherent in token exchanges and yield accrual. This buffer is mathematically calculated to ensure that the yield generated over time compensates for these losses without diluting the prize pool's integrity.

### Hook Management
The **Hook Management** system introduces a layered customization framework allowing users to embed personalized logic into the prize claiming process. This system operates on a delegate call mechanism, where pre-defined hooks (before and after prize claims) can invoke external contracts. This delegation is contingent upon gas stipulations (`HOOK_GAS` limit) to mitigate unbounded execution costs, demonstrating a careful balance between flexibility and system integrity. The hooks enable a dynamic interaction model, wherein the external contracts can execute complex operations like reinvestment strategies, conditional checks, or even external notifications, all parameterized by the winner's context and the prize details.

### Prize Claiming
The **Prize Claiming** process is central to the PoolTogether mechanism, integrating tightly with the TWAB (Time-Weighted Average Balance) system to allocate prizes fairly. The mathematical logic underpinning this system ensures that the probability of winning is proportional to the duration and amount of a participant's deposit, encapsulated in the TWAB calculation. The `claimPrize` function extends this logic by implementing checks and balances, including validation of claim eligibility and execution of user-defined hooks, ensuring that the prize distribution not only adheres to the deterministic fairness model but also accommodates for extensible engagement through external hooks.

The integration of these functionalities within PoolTogether V5 reflects a deep intertwining of deterministic logic, financial modeling, and extensible contract interactions. It underscores the project's commitment to fostering a secure, fair, and dynamic DeFi ecosystem, wherein the mathematical rigor ensures equity and the architectural design encourages user engagement and ecosystem growth.


### Time spent:
20 hours