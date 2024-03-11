# Gas-Optimization Report `pt-v5-vault`

## G-01 Use storage variables instead of re-calculating values:

In this `PrizeVault.sol`, some values are re-calculated multiple times in different functions. For example, the `totalDebt()` function calculates the total debt by calling `_totalDebt(totalSupply())`, which is also calculated in the `previewWithdraw` and `previewRedeem` functions. To save gas, the total debt can be stored in a state variable and updated whenever the total supply changes. This way, the total debt does not need to be re-calculated every time it is needed.

## G-02 Group multiple state changes together in transactions:

Functions like `deposit()`, `withdraw()`, `transferBetween()` can combine related state changes. Instead of calling deposit() then withdraw() separately, call a transfer() that batches them. This avoids the `21,000` gas base cost for each external call. Can group related account updates.

## G-03 Check allowances directly if already set:

Calling permit on ERC20 repeatedly to approve same amount wastes gas due to signature. Check `allowance()` directly before `deposit/withdraw` and only call permit if insufficient. Avoid unnecessary ERC20 calls and signatures when allowance already granted Functions like `depositWithPermit`() can check first to avoid griefing.

## G-04 Use assembly to calculate convertToAssets and convertToShares:

```solidity
function convertToAssets(uint256 _shares) public view returns (uint256) {
    assembly {
        let totalDebt := sload(_totalDebt.slot)
        let totalAssets := sload(_totalAssets.slot)

        if gt(totalAssets, totalDebt) {
            mstore(0, _shares)
            return(0, 32)
        } else {
            let assets := div(mul(_shares, totalAssets), totalDebt)
            mstore(0, assets)
            return(0, 32)
        }
    }
}

function convertToShares(uint256 _assets) public view returns (uint256) {
    assembly {
        let totalDebt := sload(_totalDebt.slot)
        let totalAssets := sload(_totalAssets.slot)

        if gt(totalAssets, totalDebt) {
            mstore(0, _assets)
            return(0, 32)
        } else {
            let shares := div(mul(_assets, totalDebt), totalAssets)
            mstore(0, shares)
            return(0, 32)
        }
    }
}
```

## G-05 Use immutable variables instead of constant:

In the `PrizeVaultFactory.sol`, `YIELD_BUFFER` is a constant variable, but it can be changed to an immutable variable to save gas. Immutable variables are cheaper than constant variables because they are only set once at the time of deployment.

## G-06 Use storage pointers instead of copying:

In the `PrizeVaultFactory::deployVault` function, the `allVaults` array is updated by pushing a new PrizeVault contract to it. Instead of creating a new instance of the `PrizeVault` contract, a storage pointer can be used to save gas. This can be done by replacing `PrizeVault _vault = new PrizeVault{...}` with `PrizeVault _vault = PrizeVault(address(new PrizeVault{...}))`.

## G-07 Use event logs instead of returning values:

In the `deployVault` function, the newly deployed `PrizeVault` contract is returned to the caller. However, returning values from a contract consumes gas. Instead, an event log can be emitted to notify the caller of the new contract address. This can be done by replacing `return _vault` with `emit VaultDeployed(_vault)`.

## G-08 Avoid using SafeCast library:

The `TwabERC20::SafeCast` library is used to cast `uint256` to `uint96` in this `TwabERC20` contract. However, this library introduces extra function calls and can be expensive in terms of gas. Instead, the uint96 value can be directly assigned to a uint256 variable without any casting. This will automatically truncate the higher order bits and will save gas.

## G-09 Avoid unnecessary state variable updates:

Every time a `state variable` is updated, it costs gas. Therefore, it is important to avoid updating state variables unnecessarily. In the `claimPrize` function, the `_hooks[_winner].useBeforeClaimPrize` and `_hooks[_winner].useAfterClaimPrize` flags are checked to determine whether to call the corresponding hooks. However, if these flags are not set, there is no need to update them. Therefore, we can add a check before updating these flags to make sure that they are not already set to the desired value.

## G-10 Use a struct to store hooks instead of a mapping:

Storing hooks in a `struct` can save gas by `reducing` the number of `storage slots` used in `HookManager.sol` contract. In this case, we can define a struct with two fields: `useBeforeClaimPrize` and `useAfterClaimPrize`, and store it in a mapping that maps addresses to structs.
