
### 1. Move require on top of function
You can check the condition of if and else statements ealier. By doing these checks first, the function is able to revert without wasting gas on executing.

*Instances (2)*:
```solidity
File: src/PrizeVault.sol

710:  else {
            revert LiquidationTokenOutNotSupported(_tokenOut);
      }

```
[Link to code](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L710)

```solidity
File: src/PrizeVault.sol

900: if (totalAssets() < totalDebt()) revert LossyDeposit(totalAssets(), totalDebt());

```
[Link to code](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L900)

### 2. State variables should be cached in stack variables rather than re-reading them from storage
State variables should be cached instead of re-reading them. Caching such variables replaces the Gwarmaccess with much cheaper stack reads.

*Instances (1)*:
```solidity
File: src/abstract/Claimable.sol

85:   if (_hooks[_winner].useBeforeClaimPrize) {
          recipient = _hooks[_winner].implementation.beforeClaimPrize{ gas: HOOK_GAS }
108:  if (_hooks[_winner].useAfterClaimPrize) {
          _hooks[_winner].implementation.afterClaimPrize{ gas: HOOK_GAS }

```
[Link to code](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L85)

## Additional instances of publicly known issues
### Using bools for storage incurs overhead.
We can also use uint256(1) and uint256(2) for struct to avoid a Gwarmaccess (100 gas).

```solidity
File: src/interfaces/IVaultHooks.sol

8:    struct VaultHooks {
            bool useBeforeClaimPrize;
            bool useAfterClaimPrize;
            IVaultHooks implementation;
      }

```
[Link to code](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol#L8)
