## Summary

### Gas Optimization

no | Issue |Instances||
|-|:-|:-:|:-:|
| [G-01] | Use assembly to emit events | 13 | - |
| [G-02] | Use hardcode address instead `address(this)` | 15 | - |
| [G-03] | Using unchecked blocks to save gasUsing unchecked blocks to save gas | 3 | - |
| [G-04] | Use assembly for math (add, sub, mul, div) | 3 | - |
| [G-05] | Not using the named return variable when a function returns, wastes deployment gas | 1 | - |
| [G-06] | Use assembly for small keccak256 hashes, in order to save gas  | 1 | - |
| [G-07] | Use assembly to validate msg.sender | 1 | - |
| [G-08] | Use Mappings Instead of Arrays | 1 | - |
| [G-09] | Sort Solidity operations using short-circuit mode | 1 | - |
| [G-10] | Before transfer of  some functions, we should check some variables for possible gas save | 2 | - |
| [G-11] | Use assembly to check for `address(0)` | 3 | - |
| [G-12] |  Using PRIVATE rather than PUBLIC FOR Immutable, Saves Gas | 2 | - |
| [G-13] | Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead | 2 | - |

## Gas Optimizations  

## [G-01] Use assembly to emit events

We can use assembly to emit events efficiently by utilizing scratch space and the free memory pointer. This will allow us to potentially avoid memory expansion costs. Note: In order to do this optimization safely, we will need to cache and restore the free memory pointer.

```solidity
file:/src/PrizeVault.sol

 562       emit Sponsor(_owner, _assets, _shares);

 621        emit ClaimYieldFeeShares(msg.sender, _shares);

 697        emit TransferYieldOut(msg.sender, _tokenOut, _receiver, _amountOut, _yieldFee);

 747        emit LiquidationPairSet(address(this), address(_liquidationPair));

 876        emit Deposit(_caller, _receiver, _assets, _shares);

 909        emit Withdraw(_caller, _receiver, _owner, _assets, _shares);

 952       emit YieldFeePercentageSet(_yieldFeePercentage);

 960        emit YieldFeeRecipientSet(_yieldFeeRecipient);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L562

```solidity
file:/src/TwabERC20.sol

 78       emit Transfer(address(0), _receiver, _amount);

 89        emit Transfer(_owner, address(0), _amount);

 102        emit Transfer(_from, _to, _amount);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/TwabERC20.sol#L78

```solidity
file:/src/abstract/Claimable.sol

131        emit ClaimerSet(_claimer);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/abstract/Claimable.sol#L131

```solidity
file:/src/abstract/HookManager.sol

31        emit SetHooks(msg.sender, hooks);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/abstract/HookManager.sol#L31


## [G-02] Use hardcode address instead `address(this)`
 
 Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address. Foundry’s script.sol and solmate’s LibRlp.sol contracts can help achieve this.
 
```solidity
file:/src/PrizeVault.sol

337        return yieldVault.convertToAssets(yieldVault.balanceOf(address(this))) + _asset.balanceOf(address
(this));

382       uint256 _latentBalance = _asset.balanceOf(address(this));

383       uint256 _maxYieldVaultDeposit = yieldVault.maxDeposit(address(this));

405        uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

416        uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

540         IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);

558        if (twabController.delegateOf(address(this), _owner) != SPONSORSHIP_ADDRESS) {

639            _maxAmountOut = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

726        return (_tokenOut == address(_asset) || _tokenOut == address(this)) && _liquidationPair == liquidationPair;

861        uint256 _assetsWithDust = _asset.balanceOf(address(this));

866        uint256 _assetsUsed = yieldVault.mint(_yieldVaultShares, address(this));

922        return yieldVault.convertToAssets(yieldVault.maxRedeem(address(this)));

936            yieldVault.redeem(_yieldVaultShares, address(this), address(this));
      
```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L337


```solidity
file:/src/TwabERC20.sol

 59       return twabController.balanceOf(address(this), _account);

 64        return twabController.totalSupply(address(this));

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/TwabERC20.sol#L59


## [G-03] Using unchecked blocks to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn't possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block.
```solidity
file:/src/PrizeVault.sol

 617        yieldFeeBalance -= _yieldFeeBalance;

 647      uint256 _liquidYield = 
 648          _availableYieldBalance(totalAssets(), _totalDebt(_totalSupply))
 649          .mulDiv(FEE_PRECISION - yieldFeePercentage, FEE_PRECISION);


 675            _yieldFee = (_amountOut * FEE_PRECISION) / (FEE_PRECISION - _yieldFeePercentage) - _amountOut;


```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L617


## [G-04] Use assembly for math (add, sub, mul, div)

Use assembly for math instead of Solidity. You can check for overflow/underflow in assembly to ensure safety. If using Solidity versions < 0.8.0 and you are using Safemath, you can gain significant gas savings by using assembly to calculate values and checking for overflow/underflow.

```solidity
file:/src/PrizeVault.sol

416         uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

617        yieldFeeBalance -= _yieldFeeBalance;

639            _maxAmountOut = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L416


## [G-05]  Not using the named return variable when a function returns, wastes deployment gas

When you execute a function that returns values in Solidity, the EVM still performs the necessary operations to execute and return those values. This includes the cost of allocating memory and packing the return values. If the returned values are not utilized, it can be seen as wasteful since you are incurring gas costs for operations that have no effect.

```solidity
file:/src/PrizeVaultFactory.sol

    ) external returns (PrizeVault) {

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVaultFactory.sol#L101



## [G-06]  Use assembly for small keccak256 hashes, in order to save gas  

If the arguments to the encode call can fit into the scratch space (two words or fewer), then it's more efficient to use assembly to generate the hash (80 gas)
```solidity
File: /src/PrizeVaultFactory.sol

 103           salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVaultFactory.sol#L103


## [G-07] Use assembly to validate `msg.sender`

```solidity
file:/src/abstract/Claimable.sol

 53       if (msg.sender != claimer) revert CallerNotClaimer(msg.sender, claimer);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/abstract/Claimable.sol#L53


## [G-08] Use Mappings Instead of Arrays

When we say "use mappings instead of arrays," we mean that when storing and accessing data in a smart contract, it's more gas-efficient to use mappings instead of arrays. This is because mappings do not require iteration to find a specific element, whereas arrays do.

```solidity
file:/src/PrizeVaultFactory.so

66   PrizeVault[] public allVaults;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVaultFactory.sol#L66



## [G-09] Sort Solidity operations using short-circuit mode

```solidity
file:/src/PrizeVault.sol

 776       if (success && encodedDecimals.length >= 32) {

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L776


## [G-10] Before transfer of  some functions, we should check some variables for possible gas save

Before transfer, we should check for amount being 0 so the function doesn't run when its not gonna do anything:

```solidity
file:/src/PrizeVault.sol

  939          _asset.transfer(_receiver, _assets);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L939

```solidity
file:/src/TwabERC20.sol

  78      emit Transfer(address(0), _receiver, _amount);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/TwabERC20.sol#L78



## [G-11] Use assembly to check for `address(0)`


```solidity
file: /src/PrizeVault.sol

 300       if (address(yieldVault_) == address(0)) revert YieldVaultZeroAddress();

 301        if (owner_ == address(0)) revert OwnerZeroAddress();
 
 743        if (address(_liquidationPair) == address(0)) revert LPZeroAddress();

```

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L300


## [G-12]  Using PRIVATE rather than PUBLIC FOR Immutable, Saves Gas        

If needed, the value can be read from the verified contract source code. Savings are due to the compiler not having to create non-payable getter functions for deployment calldata, and not adding another entry to the method ID table

```solidity
file: /src/TwabERC20.sol

 26   TwabController public immutable twabController;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/TwabERC20.sol#L26C27-L26

```solidity
file:/src/abstract/Claimable.sol

 24   PrizePool public immutable prizePool;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/abstract/Claimable.sol#L24


## [G-13] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead         

When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```solidity
file:/src/TwabERC20.sol

101        twabController.transfer(_from, _to, SafeCast.toUint96(_amount));

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/TwabERC20.sol#L101

```solidity
File:/src/abstract/Claimable.sol

 21   uint24 public constant HOOK_GAS = 150_000;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/abstract/Claimable.sol#L21
