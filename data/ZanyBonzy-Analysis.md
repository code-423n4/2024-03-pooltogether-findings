#  **Advanced Analysis Report for <img width = 12.5% src="https://www.gitbook.com/cdn-cgi/image/width=256,dpr=2,height=40,fit=contain,format=auto/https%3A%2F%2F32394911-files.gitbook.io%2F~%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MjVmQloS14HsmiACMUz-887967055%252Flogo%252FpLTDd1mFY8C8dNwLSUqG%252Fpooltogether-logo--purple-gradient.png%3Falt%3Dmedia%26token%3Dbcf4a065-3bba-4144-a7ec-a98a1bcf6142" alt="Pool together">**

## **1. Protocol Overview**

- PoolTogether is a prize-savings protocol which allows users to deposit assets, participate daily prize draws. 
- The protocol uses a prize vault, currently the 5th version which is a fully ERC4626 compliant vault that takes in asset deposits, deposits them into other yield bearing vaults and earns the yield which is randomly awarded to prizes to the users. 
- As a form of investment protection, the depositors are able to withdraw their full deposit as long as the withdrawal limits meets their balance. In case the underlysing yield bearing vault fails, users are still entitled to a fraction of their assets based on their share balance.
***
## **2. Architecture Overview**

<h3 align="center"> <b>Basic Interaction Overview</b></h3>

- The main protocol flow involves users depositing their assets into the deployed prize vault. The prize vault upon deployement will have a special yield bearing vault attached to it, in which the vaults assets will be deposited to earn yield. Some of these vaults include Yearn V3, Beefy vault, sDAI and any other ERC4626 compliant vault.

<p align="center">
    <img width= auto src="https://gist.github.com/assets/112232336/feb651d6-9893-4579-8df6-785f5634a020"
alt="Basic Interaction">
</p>

<h3 align="center"> <b>Contracts Integrations</b></h3>

- The prize vault integrates a number of other attachments that help in decentralizing its functionality and ensure smooth operations. The `TwabERC20` holds as the protocol accounting tokens which are stored in the `TwabController`, the `Liquidator` that liquidates the vaults's shares on the yield bearing vaults, and sends the rewards to the prize pool, the `Claimer` that gets claims the winner's prize from the prize pool and the `VaultHooks` and its manager that users can use perform various transactions before and after their prizes are claimed.

<p align="center">
    <img width= auto src="https://gist.github.com/assets/112232336/c8e013ec-8d66-45d0-af19-622b5050cb49"
alt="Contract interactions graph">
</p> 


<h3 align="center"> <b>Scope Overview</b></h3>

#### **[PrizeVault.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol)**

**Contract description** - The prize vault is the core of the protocol allows for asset deposits and withdrawals from users which is then translated to the yield vaults. The vault aims to be ERC4626 compliant and as such invokes its various features and also problems. One of these is the notorious rounding errors incurred by vaults on deposits and withdrawals. The protocol attempts to fix this by using the "dust collection strategy" which involves calculating the exact amount needed for the yield vault, sending that amount and leaving the leftover assets in the prize vault until they're needed for future deposits or withdrawals. The other strategy involves the use of the yield buffer which is sent to the vault upon deployment.

**User Interactions** - Users can add assets to the vaults using the `deposit` function and specifying the amount of assets they're willing to deposit, using the `mint` function and specifying the amount of shares they want minted, the `sponsor` function to deposit and delegate their assets to a sponsorship address. Users before depositing must have preapproved the vault to transfer their tokens. To make the approval and deposit process atomic, users can use the `depositWithPermit` function. On the other end of the spectrum, to remove assets, the `withdraw` and the `redeem` functions specifying the assets to withdraw or shares to burn.

**Privileged Interactions** - The owner can set the yield fee percentage on a scale of 0 to 9e8 units via the `setYieldFeePercentage` function, set the recipient of the yield fee through the `setYieldFeeRecipient` function, the liquidation pair via the `setLiquidationPair` call and set the claimer address via the `setClaimer` function. The yield fee recipient is allowed to call the `claimYieldFeeShares` to withdraw the yield fees shares. The liquidation pair verifies the tokens transferred in through the `verifyTokensIn` and liquidates the vault using the `transferTokensOut`

```
[PrizeVault, sLOC - 649]
|
|--- Inherits ----> [TwabERC20]
|                 |
|                 |--- Inherits ----> [ERC20]
|                 |                 |
|                 |                 |--- Inherits ----> [IERC20, IERC20Metadata]
|                 |                 |
|                 |                 |--- Uses ----> [SafeERC20]
|                 |
|                 |--- Inherits ----> [Claimable]
|                 |
|                 |--- Implements ----> [IERC4626, ILiquidationSource]
|
|--- Uses ----> [Math, SafeERC20]
|
|--- Interacts ----> [IERC4626 (yieldVault)]
|                 |
|                 |--- Inherits ----> [IERC20]
|
|--- Interacts ----> [PrizePool]
|
|--- Interacts ----> [TwabController]
|
|--- Interacts ----> [ILiquidationSource]
|
|--- Events ----> [YieldFeeRecipientSet, YieldFeePercentageSet, Sponsor, TransferYieldOut, ClaimYieldFeeShares]
|
|--- Modifiers ----> [onlyLiquidationPair, onlyYieldFeeRecipient]
|
|--- Errors ----> [YieldVaultZeroAddress, OwnerZeroAddress, WithdrawZeroAssets, BurnZeroShares, DepositZeroAssets, MintZeroShares, ZeroTotalAssets, LPZeroAddress, SweepZeroAssets, LiquidationAmountOutZero, CallerNotLP, CallerNotYieldFeeRecipient, PermitCallerNotOwner, YieldFeePercentageExceedsMax, SharesExceedsYieldFeeBalance, LiquidationTokenInNotPrizeToken, LiquidationTokenOutNotSupported, LiquidationExceedsAvailable, LossyDeposit]
|
|
[PrizeVault]
|
| - FEE_PRECISION: uint32                                
| - MAX_YIELD_FEE: uint32                                
| - yieldBuffer: uint256                                 
| - yieldVault: IERC4626                                 
| - yieldFeePercentage: uint32                           
| - yieldFeeRecipient: address                           
| - yieldFeeBalance: uint256                             
| - liquidationPair: address                             
| - _asset: IERC20                                       
| - _underlyingDecimals: uint8
|
|---Constructor
| + constructor(name_, symbol_, yieldVault_, prizePool_, claimer_, yieldFeeRecipient_, yieldFeePercentage_, yieldBuffer_, owner_)
|
|---Public/External functions
|--> decimals(): uint8                                    
|--> asset(): address                                     
|--> totalAssets(): uint256                               
|--> convertToShares(uint256): uint256                    
|--> convertToAssets(uint256): uint256                    
|--> maxDeposit(address): uint256                         
|--> maxMint(address): uint256                            
|--> maxWithdraw(address): uint256                        
|--> maxRedeem(address): uint256                          
|--> previewDeposit(uint256): uint256                     
|--> previewMint(uint256): uint256                        
|--> previewWithdraw(uint256): uint256                    
|--> previewRedeem(uint256): uint256                      
|--> deposit(uint256, address): uint256                   
|--> mint(uint256, address): uint256                      
|--> withdraw(uint256, address, address): uint256         
|--> redeem(uint256, address, address): uint256          
|--> depositWithPermit(uint256, address, uint256, uint8, bytes32, bytes32): uint256                        
|--> sponsor(uint256): uint256                           
|--> totalDebt(): uint256                                
|--> totalYieldBalance(): uint256                        
|--> availableYieldBalance(): uint256                    
|--> currentYieldBuffer(): uint256                       
|--> claimYieldFeeShares(uint256): void                  
|--> liquidatableBalanceOf(address): uint256             
|--> transferTokensOut(address, address, address, uint256): bytes
|--> verifyTokensIn(address, uint256, bytes): void        
|--> targetOf(address): address                           
|--> isLiquidationPair(address, address): bool            
|--> setClaimer(address): void                            
|--> setLiquidationPair(address): void                    
|--> setYieldFeePercentage(uint32): void                 
|--> setYieldFeeRecipient(address): void
|
|-Internal functions
|
|---> _tryGetAssetDecimals(IERC20): (bool, uint8)         
|---> _totalDebt(uint256): uint256                        
|---> _twabSupplyLimit(uint256): uint256                  
|---> _totalYieldBalance(uint256, uint256): uint256       
|---> _availableYieldBalance(uint256, uint256): uint256   
|---> _depositAndMint(address, address, uint256, uint256): void 
|---> _burnAndWithdraw(address, address, address, uint256, uint256): void 
|---> _maxYieldVaultWithdraw(): uint256                    
|---> _withdraw(address, uint256): void                    
|---> _setYieldFeePercentage(uint32): void                 
|---> _setYieldFeeRecipient(address): void                 
|
|
```

#### **[PrizeVaultFactory.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol)**

**Contract description** - This is the factory contract that deploys and stores the ERC4626 prize vaults using CREATE2 opcode.

**User Interactions** - Users call the `deployVault` function to deploy their vault, passing in the various important parameters and characteristics. To protect from first depositor issue, a small, irrecoverable amount of the underlying assets (the yield buffer) must be sent to the to the vault upon deployment. Upon deployment, the vault is added to the list of protocol vaults.

```
                                    +-------------------------------------------------+
                                    |          PrizeVaultFactory, sLOC - 55           |
                                    +-------------------------------------------------+
                                    | - YIELD_BUFFER: uint256                         |
                                    | - allVaults: PrizeVault[]                       |
                                    | - deployedVaults: mapping(address => bool)      |
                                    | - deployerNonces: mapping(address => uint256)   |
                                    +-------------------------------------------------+
                                    | + deployVault(...)                              |
                                    |   - _name: string                               |
                                    |   - _symbol: string                             |
                                    |   - _yieldVault: IERC4626                       |
                                    |   - _prizePool: PrizePool                       |
                                    |   - _claimer: address                           |
                                    |   - _yieldFeeRecipient: address                 |
                                    |   - _yieldFeePercentage: uint32                 |
                                    |   - _owner: address                             |
                                    | + totalVaults(): uint256                        |
                                    +-------------------------------------------------+

```


#### **[TwabERC20.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol)**

**Contract description** - This is the accounting ERC20 token that stores protocol balances in the TwabController. It enables the caluclation of of time-weighted average balances for each protocol user. It also facilitates composability eith the protocol's prize pool. It aims to follow the ERC-20 standard.

**User Interactions** - Users can `mint`, `burn`, `approve` and `transfer` their tokens. These operations, except from approvals, are all sent to the TwabController contract for exexcution.

```
                        +--------------------------------------------------------------------------------+
                        |                               TwabERC20, sLOC - 57                             |
                        +--------------------------------------------------------------------------------+
                        |                                                                                |
                        |   +----------------+         +----------------+         +------------------+   |
                        |   | TwabController |<--------| TwabERC20     |<--------| ERC20, ERC20Permit|   |
                        |   +----------------+         +----------------+         +------------------+   |
                        |                                                                                |
                        | Constructor:                                                                   |
                        | - name_: string                                                                |
                        | - symbol_: string                                                              |
                        | - twabController_: TwabController                                              |
                        |                                                                                |
                        | Functions:                                                                     |
                        | - balanceOf(_account: address): uint256                                        |
                        | - totalSupply(): uint256                                                       |
                        | - _mint(_receiver: address, _amount: uint256)                                  |
                        | - _burn(_owner: address, _amount: uint256)                                     |
                        | - _transfer(_from: address, _to: address, _amount: uint256)                    |
                        |                                                                                |
                        | Events:                                                                        |
                        | - Transfer(from: address, to: address, amount: uint256)                        |
                        |                                                                                |
                        +--------------------------------------------------------------------------------+

```

#### **[Claimable.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol)**

**Contract description** - This contract allows for claiming of user's prices from the prizepool to earn fees. The process is performed the owner defined claimer which routinely calls the `claimPrize` function while forwarding the transaction to the prizepool. The process also goes through the hooks in use by the prize winner, which can be used to perform other external transactions.


```
                            +-----------------------------------------------------------+
                            |                                                           |
                            |                  Claimable, sLOC - 66                     |
                            |                                                           |
                            | +-------------------------------------------------------+ |
                            | | Constructor                                           | |
                            | | ------------------------------------------------------| |
                            | | | - prizePool_: PrizePool                             | |
                            | | | - claimer_: address                                 | |
                            | | |                                                     | |
                            | | +-----------------------------------------------------+ |
                            |                                                           |
                            | +-------------------------------------------------------+ |
                            | | claimPrize                                            | |
                            | | +-----------------------------------------------------+ |
                            | | | - _winner: address                                  | |
                            | | | - _tier: uint8                                      | |
                            | | | - _prizeIndex: uint32                               | |
                            | | | - _reward: uint96                                   | |
                            | | | - _rewardRecipient: address                         | |
                            | | |                                                     | |
                            | | | +-------Hooks-------------------------------------+ | |
                            | | | | - beforeClaimPrize                              | | |
                            | | | | - prizePool.claimPrize                          | | |
                            | | | | - afterClaimPrize                               | | |
                            | | | +-------------------------------------------------+ | |
                            | | +-----------------------------------------------------+ |
                            |                                                           |
                            | +-------------------------------------------------------+ |
                            | | _setClaimer                                           | |
                            | | ------------------------------------------------------| |
                            | | | - _claimer: address                                 | |
                            | | +-----------------------------------------------------+ |
                            |                                                           |
                            +-----------------------------------------------------------+

```


#### **[HookManager.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol)**

**Contract description** - The HookManager is an abstract, base contract, through which users can set and manage their prize hooks. 

**User Interactions** - By calling the `setHooks` function, users can have their hooks and its `IVaultHooks` characteristics set and ready to use upon prize claim. Important to note that while hooks can be set, they can't be removed or disabled.

```
                                            +------------------------+
                                            | HookManager, sLOC - 13 |
                                            | +--------------------+ | 
                                            | | getHooks(address)  | | 
                                            | +--------------------+ | 
                                            | +--------------------+ | 
                                            | | setHooks(hooks)    | | 
                                            | +--------------------+ |
                                            +------------------------+

```

#### **[IVaultHooks.sol](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol)**

**Contract description** - THe contract functions as an default interface for users looking to implement their prize hooks on a vault. It holds a struct that defines the hook contract's various characteristics from which the `beforeClaimPrize` and `afterClaimPrize` functions can be called upon users claiming their prizes.

```
                                    VaultHooks, sLOC - 22
                                    |
                                    |--> useBeforeClaimPrize: bool
                                    |
                                    |--> useAfterClaimPrize: bool
                                    |
                                    |--> IVaultHooks implementation
                                    |
                                    IVaultHooks
                                    |
                                    |--> beforeClaimPrize(address winner, uint8 tier, uint32 prizeIndex, uint96 reward, address rewardRecipient) returns (address)
                                    |
                                    |--> afterClaimPrize(address winner, uint8 tier, uint32 prizeIndex, uint256 prize, address recipient)

```

***

## **3. Codebase Overview**

- **Audit Information** - For the purpose of this audit, the PoolTogether v5 codebase comprises of six smart contracts with a total of 648 SLoC. It holds two external imports, 30 external calls, seven seperate interface and struct definitions. Its core design principle is composition and holds a number of integrations with other vault types, enabling efficient and flexible integration. It exists as an rewrite of the previous versions and is scheduled to be deployed to the Optimism, Arbitrum, Base and other L2 chains, giving each user the freedom to transact in and across all platforms. 

- **External Integrations** - The codebase is to be integrated with the broader PoolTogether network with interactions with the protocol's PrizePool, TwabController, LiquidationPair and Claimer codebases. As with external defi, mostly ERC4626 compliant vaults, most of which include Yearn V3, Beefy, sDAI, Yield Daddy Aave V3 Wrapper, Yield Daddy Lido Wrapper and so on. 

- **Attack Vectors** - As requested by the sponsors, strict EIP compliance and issues from integrations with other yield vaults are good places to begin the hack. Gaming yields, user/protocol griefing, slippage issues, inflation vectors, denial of service and so on are other very important points of attack.

- **Token Support** - The protocol works with the various ERC20 tokens. In return, users get the protocol's TwabERC20 token which helps with protcol accounting. The contracts implementations is mostly suited for most token types as the safeERC20 functions are implemented, except for rebasing tokens. The exceptions are fee-on-transfer tokens and tokens that have low precision to dollar value ratio like WBTC. 

- **Testability** - Test coverage is about 99% which is very good. The implemented tests include unit test which tests the basic functionalities of the functions and helped with basic bugs. Fuzz tests, invariant tests and integration tests with other defi vaults which provided the much needed basic securities.

- **Documentation and NatSpec** - The provided docs and readme are pretty good as they provide a both technical and layman breakdown of the contracts and their intended uses. There's an important video for the protocol walkthrough and guides on how to user it contract. The contracts are also well commented, not strictly to NatSpec but each function was well explained. It made the audit process easier.

- **Error handling and Event Emission** - Custom errors were defined and in use in the codebase, marking a point of gas efficiency, albeit most of them lack error messages. Events are well handled, emitted for important parameter changes, although in some cases, they seemed to not follow the checks-effects-interaction patterns. 

***

## **4. Protocol Risks** 

- **Centralization** - While for the most part, the contracts aim to be autonomous, there are still some privileged functions to which the protocol and its users are vulnerable including deploying the vaults with faulty parameters that can grief users, liquidating an otherwise healthy vault, setting wrong claimer and liquidation pair addresses and so on.

- **Systemic Risks** 

     1. Lack of a way to close or pause vault functionality, as deployed vaults cannot be removed from list of vaults.
     2. Lack of a sweep function in the contracts can lead to loss of tokens that cannot be recovered.
     3. Malicious users looking to grief other users or steal rewards.
     4. Issues with external integrations, compiler issues, inherited contracts.
     5. Vulnerabilities from other smart contracts, which at best may be contained in the erring contracts, at worst affect the whole protocol.
     6. Network issues, opcodes and scalability issues.
***
## **5. Audit Recommendations**

- The codebase should be sanitized, and the comments should also be brought up to date to conform with NatSpec.

- Some tokens have variable balances, balance checks before and after these transfers can help track the balance changes so as to prevent accounting issues.

- In certain cases, the contracts TwabERC20 and PrizeVault do not fully comply to the expected token standards, this is mostly due to their integrations with external contracts. These cases should be mitigated against.

- Contrary to the docs, and comments, users can't deploy vaults without claimers, for this a bit of refactoring of the setClaimer functions will be required. Or, the docs updated to fit this new information.

- The codebase uses mostly unsafe ERC20 methods to handle token transfer. Safer methods should used instead as they handle a wider array of tokens including non-standard ones.
    
- A pause functionality can be of great help for the vault in cases of vault compromise. Note that only functions to add assets to the vaults should be pausable, functions to remove assets should be free.

- Speaking of vault compromise, there's no way to remove vaults, this can pose great risk to users as malicious vaults can be used to honeypot users into losing their funds.

- Two step variable and address updates should be implemented. Most of the setter functions implement the changes almost immediately which can be jarring for the contracts/users. Consider introducing timelocks. These fixes can help protect from mistakes and unexepected behaviour.
    
- Finally, reported issues and bugs should be mitigated against.
***
## **6. Resources**

- [C4 ReadMe](https://code4rena.com/audits/2024-03-pooltogether#top)
- [Code Repository](https://github.com/GenerationSoftware/pt-v5-vault/tree/94b0c034c68b5318a25211a7b9f6d9ff6693e6ab)
- [Technical Documentation](https://dev.pooltogether.com/)
- [Website](https://pooltogether.com/)
- [Video Walkthrough](https://ipfs.io/ipfs/bafybeigjlvmcieuc3j7lypxg2653jaxukbnsbj44kcvqsukfyt6sgskhgm)

### Time spent:
30 hours