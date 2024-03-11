## Overview
The pooltogether v5 prizevault contract protocol that offers a no loss prize saving system. Maintaining a core principle of no loss and introducing new features such as; creating vault for new assets, arbitraging yield for pool tokens and earning fees by claiming prizes.
User can deposit asset into a prize vault and swap for Pooltogether prize token, earn interest over time and have the interest converted into pool tokens for prize distribution

## Modifier Roles

Some roles that can be only called by priviledged

- The `onlyLiquidationPair` : the caller to be the liquidation pair.
```solidity
    modifier onlyLiquidationPair() {
        if (msg.sender != liquidationPair) {
            revert CallerNotLP(msg.sender, liquidationPair);
        }
        _;
    }
```

- The `onlyYielfFeeRecipient` : equires if the caller is the yield fee recipient.
```solidity
    modifier onlyYieldFeeRecipient() {
        if (msg.sender != yieldFeeRecipient) {
            revert CallerNotYieldFeeRecipient(msg.sender, yieldFeeRecipient);
        }
        _;
    }
```

## Architecture
The pooltogether v5 protocol use two main system structure
- The PrizeVault Factory
- The PrizeVault 

![The Architecture][https://drive.google.com/file/d/11yTuaTXQx5wxQY6Wg05QjUCWmPvuVqkR/view?usp=sharing]

The `PrizeVaultFactory` is used to deploy new prize vaults using the standard ERC4656 underlying yield vault. It implements the `yield buffer` strategy which is one of the two methods used in tackling rounding error in the v5 `prizeVault`. The `YIELD_BUFFER` is an invariant deployed by the factory `1e5`. What it mean, is deploying a vault with an underlying asset, the deployer will have to approve the factory to spend an insignificant value which is used to cover rounding error upon deposit and withdrawal to be sent to the `prizeVault` during deployment.

Futher the factory contract is used to keep track of the vaults deployed by the factory and also verify if the factory deployed the vault

The `prizeVault` is designed to follow a structure "no loss" spirit in the poolTogether protocol down to the last wei. The design of the prizeVault is aimed at taking deposit from user into the vault for shares in return for minting the twabERC20 token to user, and accumulating/funneling the deposit of users into a yieldVault for generating yield and then the yield generated is being liquidated and sent to the prize pool which is then converted to prize token available for claimer to win.

The prizeVault implements two strategy in protecting users against bad actors and also giving the means to withdraw all their deposit. One  often common issue in the vault(rounding errors). The poolTogether v5 uses a `YIELD_BUFFER` and `dust collection`.

- The `dust strategy` is used when a user deposit figure that may incure a decimal when trying to mint shares and to note solidity doesn't support floating numbers. The poolTogether v5 uses this strategy to make sure that user deposits are rounded down to the actual fixed figure that can be calculated and minted to the vault.

### How it works?
Users are to deposit underlying asset i.e USDC to the vault and return gets a share which is a twabERC20 token is minted to the user, If a user deposit an amount that will incure into decimals(not 1:1 ratio), the protocol round down and calculates the amount to mint in fixed, returning the remaining amount to the prizeVault as the `latent balance`.

- The `YIELD_BUFFER` is used to ensure that there is always enough yield to cover the rounding error of withdrawal and deposit. The buffer prevents the entire balance of to be liquidated which can potentially leave the vault in a state where the rounding error can reduce the `totalAsset` to be lesser than the `totalSupply` and in such instance goes against the pooltogether protocol theme "no loss"



## Systemic and centralization Risk

As for decentralizated exchange protocol. there are always risk systemic risk involved
- The prixeVault can be exposed to potential bugs when accumulating the deposit of users into the vault
- The dust strategy can also have potential vulnerability, reasons are it takes the balance of a deposit and pushes it to the yield vault after some calculations made then rest are stored in the latent balance. 
Recommendation: To probably create a function that returns balance of the user after round down as taken place and the needed amount has gone into the vaults


### Centralization 
- The use of liquidation of the yieldVault which can only e done by an assigned address. This could potentiallyhave  attempt to manipulate the Prize Pool winners to reward the users or even a bad actor taking charge of the owner contract.

- The protocol has made efforts to eliminate every possible instance where a protocol can  be exploited through admin roles and it very commended.

## Approach taken to evaluate codebase
During the timeframe of prizeVault pooltogether audit, my main focus was on examining the prizevault system and the factory system in the Pooltogether V5 protocol.
- I spent time understanding the overall working of the Pooltogether V5 system and getting an overview of the codebase also how it implements the underlying yieldVault to the PrizeVault
- I conducted a detailed exploration of the Liquidation and Vault.

- Then dedicated the day to preparing the analysis, summarizing and recommendations.

## Conclusion

PoolTogether team did a great job in pulling this up especially the `prizeVault` contract which an extensive contract that has great system design for managing assest, yield generation and earning, participating in prize pools and handling liquidation firmly.



### Time spent:
8 hours