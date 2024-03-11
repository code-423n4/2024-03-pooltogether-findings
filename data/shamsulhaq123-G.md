## Gas Optimization

## Summary

|No|Issue|Instance|
|--|-----|--------|
|[G-01]| address(this) can be stored in state variable that will ultimately cost less|20|
|[G-02]|Call msg.sender directly instead of caching them|14|
|[G-03]|Use assembly to check for `address(0)`|8|
|[G-04]|+= costs more gas than = + for state variables|1|
|[G-05]|Use assembly to emit events|14|
|[G-06]|Stack variable cost less while used in emiting event|14|
|[G-07]|Use constants instead of type(uintx).max|2|
|[G-08]| Use hardcode address instead address(this)|20|
|[G-09]|Modifiers are redundant if used only once or not used at all.|1|
|[G-10]|Use function instead of modifiers |2|
|[G-11]|Remove or replace unused state variables|3|
|[G-12]|Should use arguments instead of state variable |14|
|[G-13]|Stack variable cost less while used in emiting event|14|
|[G-14]| Do not calculate constant|4|
|[G-15]| Using private rather than public for constants, saves gas|4|
|[G-16]|Use assembly in place of abi.decode to extract calldata values more efficiently|1|
|[G-17]|Use of Custom Errors Instead of String|2|
|[G-18]|Using XOR (^) and AND (&) bitwise equivalents for gas optimizations|2|
|[G-19]|Using this to access functions results in an external call, wasting gas|20|

### [G-01] address(this) can be stored in state variable that will ultimately cost less
than every time calculating this specifically when it used multiple times in a contract

```solidity

file: PrizeVault.sol

337    return yieldVault.convertToAssets(yieldVault.balanceOf(address(this))) + _asset.balanceOf(address(this));

382  uint256 _latentBalance = _asset.balanceOf(address(this));
383   uint256 _maxYieldVaultDeposit = yieldVault.maxDeposit(address(this));

405    uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

416  uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

539   if (_asset.allowance(_owner, address(this)) != _assets) {
540    IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s

558   if (twabController.delegateOf(address(this), _owner) != SPONSORSHIP_ADDRESS) 

634   if (_tokenOut == address(this))

639 _maxAmountOut = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

691  else if (_tokenOut == address(this)) 

713    prizePool.contributePrizeTokens(address(this), _amountIn);

726  return (_tokenOut == address(_asset) || _tokenOut == address(this)) && _liquidationPair == liquidationPair;  

747  emit LiquidationPairSet(address(this), address(_liquidationPair));

856     address(this),

861   uint256 _assetsWithDust = _asset.balanceOf(addres

866  uint256 _assetsUsed = yieldVault.mint(_yieldVaultShares, address(this));

922   return yieldVault.convertToAssets(yieldVault.maxRedeem(address(this)));

931  uint256 _latentAssets = _asset.balanceOf(address(this));

936  yieldVault.redeem(_yieldVaultShares, address(this), address(this));

938  if (_receiver != address(this))
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L337
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L382-L383
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L405
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L416
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L539-L540
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L558
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L634
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L639
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L691
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L713
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L726
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L747
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L856
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L861
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L866
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L922
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L931
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L936
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L938

```solidity

file: TwabERC20.sol

59  return twabController.balanceOf(address(this), _account);

64  return twabController.totalSupply(address(this));
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L59
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L64

### [G-02] Call msg.sender directly instead of caching them

In the instance below, instead of caching `msg.sender` and incurring unnecessary stack manipulation, we can call `msg.sender` directly.

```solidity

file: PrizeVault.sol

261   if (msg.sender != liquidationPair) {
262    revert CallerNotLP(msg.sender, liquidationPair);

269   if (msg.sender != yieldFeeRecipient) {
270    revert CallerNotYieldFeeRecipient(msg.sender, yieldFeeRecipient);

477  _depositAndMint(msg.sender, _receiver, _assets, _shares);

484  _depositAndMint(msg.sender, _receiver, _assets, _shares);

495  _burnAndWithdraw(msg.sender, _receiver, _owner, _shares, _assets);

506 _burnAndWithdraw(msg.sender, _receiver, _owner, _shares, _assets);

532  if (_owner != msg.sender) {
            revert PermitCallerNotOwner(msg.sender, _owner);

553  address _owner = msg.sender; 

619 _mint(msg.sender, _shares);
621     emit ClaimYieldFeeShares(msg.sender, _shares);

697     emit TransferYieldOut(msg.sender, _tokenOut, _receiver, _amountOut, _yieldFee);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L261-L262
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L269-L270
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L477
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L484
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L495
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L506
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L532-L533
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L553
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L619-L621
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L697

```solidity

file: PrizeVaultFactory.sol

103  salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))

118  IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L103
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L118

```solidity

file: abstract/Claimable.sol

53    if (msg.sender != claimer) revert CallerNotClaimer(msg.sender, claimer);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L53

```solidity

file: abstract/HookManager.sol

30   _hooks[msg.sender] = hooks;
31     emit SetHooks(msg.sender, hooks);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L30-L31

### [G-03]  Use assembly to check for `address(0)`

```solidity

file: PrizeVault.sol

300  if (address(yieldVault_) == address(0)) revert YieldVaultZeroAddress();
        if (owner_ == address(0)) revert OwnerZeroAddress();

743  if (address(_liquidationPair) == address(0)) revert LPZeroAddress();        
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L300-L301
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L743

```solidity

file: TwabERC20.sol

47  if (address(0) == address(twabController_)) revert TwabControllerZeroAddress();

78   emit Transfer(address(0), _receiver, _amount);

89  emit Transfer(_owner, address(0), _amount);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L47
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L78
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L89

```solidity

file: abstract/Claimable.so

65  if (address(prizePool_) == address(0)) revert PrizePoolZeroAddress();

97  if (recipient == address(0)) revert ClaimRecipientZeroAddress();

129    if (_claimer == address(0)) revert ClaimerZeroAddress();

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L65
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L97
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L129

## [G-04] += costs more gas than = + for state variables
use = + or = - instead to save gas

```solidity

file: PrizeVault.sol

685   yieldFeeBalance += _yieldFee;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L685

## [G-05] Use assembly to emit events
We can use assembly to emit events efficiently by utilizing `scratch space` and the `free memory pointer`. This will allow us to potentially avoid memory expansion costs. Note: In order to do this optimization safely, we will need to cache and restore the free memory pointer.

```solidity

file: PrizeVault.sol

562   emit Sponsor(_owner, _assets, _shares);

621   emit ClaimYieldFeeShares(msg.sender, _shares)

697   emit TransferYieldOut(msg.sender, _tokenOut, _receiver, _amountOut, _yieldFee);

747   emit LiquidationPairSet(address(this), address(_liquidationPair));

876  emit Deposit(_caller, _receiver, _assets, _shares);

909  emit Withdraw(_caller, _receiver, _owner, _assets, _shares);

952  emit YieldFeePercentageSet(_yieldFeePercentage);

960   emit YieldFeeRecipientSet(_yieldFeeRecipient);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L562
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L621
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L697
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L747
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L876
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L909
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L952
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L960

```solidity

file: PrizeVaultFactory.sol

123 
        emit NewPrizeVault(
            _vault,
            _yieldVault,
            _prizePool,
            _name,
            _symbol
        );

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L123

```solidity

file: TwabERC20.sol

78   emit Transfer(address(0), _receiver, _amount);

89     emit Transfer(_owner, address(0), _amount);

102 emit Transfer(_from, _to, _amount);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L78
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L89
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L102

```solidity

file: abstract/Claimable.sol

131    emit ClaimerSet(_claimer);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L131

```solidity

file: abstract/HookManager.sol

30  emit SetHooks(msg.sender, hooks);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L30

## [G-06] Stack variable cost less while used in emiting event
Even if the variable is going to be used only one time, caching a state variable and use its cache in an emit would help you reduce the cost by at least

```solidity

file: PrizeVault.sol

562   emit Sponsor(_owner, _assets, _shares);

621   emit ClaimYieldFeeShares(msg.sender, _shares)

697   emit TransferYieldOut(msg.sender, _tokenOut, _receiver, _amountOut, _yieldFee);

747   emit LiquidationPairSet(address(this), address(_liquidationPair));

876  emit Deposit(_caller, _receiver, _assets, _shares);

909  emit Withdraw(_caller, _receiver, _owner, _assets, _shares);

952  emit YieldFeePercentageSet(_yieldFeePercentage);

960   emit YieldFeeRecipientSet(_yieldFeeRecipient);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L562
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L621
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L697
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L747
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L876
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L909
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L952
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L960

```solidity

file: PrizeVaultFactory.sol

123 
        emit NewPrizeVault(
            _vault,
            _yieldVault,
            _prizePool,
            _name,
            _symbol
        );

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L123

```solidity

file: TwabERC20.sol

78   emit Transfer(address(0), _receiver, _amount);

89     emit Transfer(_owner, address(0), _amount);

102 emit Transfer(_from, _to, _amount);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L78
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L89
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L102

```solidity

file: abstract/Claimable.sol

131    emit ClaimerSet(_claimer);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L131

```solidity

file: abstract/HookManager.sol

30  emit SetHooks(msg.sender, hooks);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L30

## [G-07] Use constants instead of type(uintx).max

type(uint120).max or type(uint128).max, etc. it uses more gas in the distribution process and also for each transaction than constant usage.

```solidity

file: PrizeVault.sol

778   if (returnedDecimals <= type(uint8).max) 

800    return type(uint96).max - _totalSupply;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L778
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L800

## [G-08] Use hardcode address instead address(this)
Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address.

```solidity

file: PrizeVault.sol

337    return yieldVault.convertToAssets(yieldVault.balanceOf(address(this))) + _asset.balanceOf(address(this));

382  uint256 _latentBalance = _asset.balanceOf(address(this));
383   uint256 _maxYieldVaultDeposit = yieldVault.maxDeposit(address(this));

405    uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

416  uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

539   if (_asset.allowance(_owner, address(this)) != _assets) {
540    IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s

558   if (twabController.delegateOf(address(this), _owner) != SPONSORSHIP_ADDRESS) 

634   if (_tokenOut == address(this))

639 _maxAmountOut = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

691  else if (_tokenOut == address(this)) 

713    prizePool.contributePrizeTokens(address(this), _amountIn);

726  return (_tokenOut == address(_asset) || _tokenOut == address(this)) && _liquidationPair == liquidationPair;  

747  emit LiquidationPairSet(address(this), address(_liquidationPair));

856     address(this),

861   uint256 _assetsWithDust = _asset.balanceOf(addres

866  uint256 _assetsUsed = yieldVault.mint(_yieldVaultShares, address(this));

922   return yieldVault.convertToAssets(yieldVault.maxRedeem(address(this)));

931  uint256 _latentAssets = _asset.balanceOf(address(this));

936  yieldVault.redeem(_yieldVaultShares, address(this), address(this));

938  if (_receiver != address(this))
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L337
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L382-L383
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L405
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L416
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L539-L540
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L558
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L634
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L639
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L691
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L713
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L726
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L747
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L856
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L861
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L866
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L922
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L931
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L936
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L938

```solidity

file: TwabERC20.sol

59  return twabController.balanceOf(address(this), _account);

64  return twabController.totalSupply(address(this));
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L59
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L64

## [G-09] Modifiers are redundant if used only once or not used at all. 

```solidity

file: PrizeVault.sol

260   modifier onlyLiquidationPair() 
268    modifier onlyYieldFeeRecipient()
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L260-L268

## [G-10] Use function instead of modifiers 

```solidity

file: PrizeVault.sol

260   modifier onlyLiquidationPair() 
268    modifier onlyYieldFeeRecipient()
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L260-L268

```solidity

file: abstract/Claimable.sol

52  modifier onlyClaimer() 

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L52

## [G-11] Remove or replace unused state variables

```solidity

file: PrizeVaultFactory.sol

69 mapping(address vault => bool deployedByFactory) public deployedVaults;

72   mapping(address deployer => uint256 nonce) public deployerNonces;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L69
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L72

```solidity

file: abstract/HookManager.sol

17   mapping(address => VaultHooks) internal _hooks;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L17

## [G-12] Should use arguments instead of state variable 

state variables should not used in emit  ,  This will save near 97 gas  

```solidity

file: PrizeVault.sol

562   emit Sponsor(_owner, _assets, _shares);

621   emit ClaimYieldFeeShares(msg.sender, _shares)

697   emit TransferYieldOut(msg.sender, _tokenOut, _receiver, _amountOut, _yieldFee);

747   emit LiquidationPairSet(address(this), address(_liquidationPair));

876  emit Deposit(_caller, _receiver, _assets, _shares);

909  emit Withdraw(_caller, _receiver, _owner, _assets, _shares);

952  emit YieldFeePercentageSet(_yieldFeePercentage);

960   emit YieldFeeRecipientSet(_yieldFeeRecipient);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L562
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L621
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L697
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L747
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L876
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L909
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L952
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L960

```solidity

file: PrizeVaultFactory.sol

123 
        emit NewPrizeVault(
            _vault,
            _yieldVault,
            _prizePool,
            _name,
            _symbol
        );

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L123

```solidity

file: TwabERC20.sol

78   emit Transfer(address(0), _receiver, _amount);

89     emit Transfer(_owner, address(0), _amount);

102 emit Transfer(_from, _to, _amount);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L78
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L89
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L102

```solidity

file: abstract/Claimable.sol

131    emit ClaimerSet(_claimer);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L131

```solidity

file: abstract/HookManager.sol

30  emit SetHooks(msg.sender, hooks);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L30

## [G-13] Stack variable cost less while used in emiting event
Even if the variable is going to be used only one time, caching a state variable and use its cache in an emit would help you reduce the cost by at least

```solidity

file: PrizeVault.sol

562   emit Sponsor(_owner, _assets, _shares);

621   emit ClaimYieldFeeShares(msg.sender, _shares)

697   emit TransferYieldOut(msg.sender, _tokenOut, _receiver, _amountOut, _yieldFee);

747   emit LiquidationPairSet(address(this), address(_liquidationPair));

876  emit Deposit(_caller, _receiver, _assets, _shares);

909  emit Withdraw(_caller, _receiver, _owner, _assets, _shares);

952  emit YieldFeePercentageSet(_yieldFeePercentage);

960   emit YieldFeeRecipientSet(_yieldFeeRecipient);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L562
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L621
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L697
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L747
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L876
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L909
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L952
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L960

```solidity

file: PrizeVaultFactory.sol

123 
        emit NewPrizeVault(
            _vault,
            _yieldVault,
            _prizePool,
            _name,
            _symbol
        );

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L123

```solidity

file: TwabERC20.sol

78   emit Transfer(address(0), _receiver, _amount);

89     emit Transfer(_owner, address(0), _amount);

102 emit Transfer(_from, _to, _amount);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L78
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L89
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L102

```solidity

file: abstract/Claimable.sol

131    emit ClaimerSet(_claimer);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L131

```solidity

file: abstract/HookManager.sol

30  emit SetHooks(msg.sender, hooks);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L30

## [G-14] Do not calculate constant
When you define a constant in Solidity, the compiler can calculate its value at compile-time and replace all references to the constant with its computed value. This can be helpful for readability and reducing the size of the compiled code, but it can also increase gas usage at runtime.

```solidity

file: PrizeVault.sol

74  uint32 public constant FEE_PRECISION = 1e9;

80  uint32 public constant MAX_YIELD_FEE = 9e8;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L74
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L80

```solidity

file: PrizeVaultFactory.sol

63 uint256 public constant YIELD_BUFFER = 1e5;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L63

```solidity

file: abstract/Claimable.sol

21  uint24 public constant HOOK_GAS = 150_000;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L21

## [G-15] Using private rather than public for constants, saves gas

```solidity

file: PrizeVault.sol

74  uint32 public constant FEE_PRECISION = 1e9;

80  uint32 public constant MAX_YIELD_FEE = 9e8;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L74
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L80

```solidity

file: PrizeVaultFactory.sol

63 uint256 public constant YIELD_BUFFER = 1e5;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L63

```solidity

file: abstract/Claimable.sol

21  uint24 public constant HOOK_GAS = 150_000;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L21

## [G-16] Use assembly in place of abi.decode to extract calldata values more efficiently
Instead of using abi.decode we can use assembly to decode our desired calldata values directly. This will allow us to avoid decoding calldata values that we will not use. we can use assembly to decode our desired calldata values directly. This will allow us to avoid decoding calldata values that we will not use

```solidity

file: PrizeVault.sol

777  uint256 returnedDecimals = abi.decode(encodedDecimals, (uint256));

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L777

## [G-17] Use of Custom Errors Instead of String
To save some gas the use of custom errors leads to cheaper deploy time cost and run time cost. The run time cost is only relevant when the revert condition is met.

```solidity

file: PrizeVaultFactory.sol

4 import { IERC20, IERC4626 } from "openzeppelin/token/ERC20/extensions/ERC4626.sol";

import { PrizePool } from "pt-v5-prize-pool/PrizePool.sol";

8 import { PrizeVault } from "./PrizeVault.sol";
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L4

```solidity

file: TwabERC20.sol

4 import { ERC20 } from "openzeppelin/token/ERC20/ERC20.sol";
import { ERC20Permit } from "openzeppelin/token/ERC20/extensions/ERC20Permit.sol";
import { SafeCast } from "openzeppelin/utils/math/SafeCast.sol";

8 import { TwabController } from "pt-v5-twab-controller/TwabController.sol";
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L4-L8

## [G-18] Using XOR (^) and AND (&) bitwise equivalents for gas optimizations
Given 4 variables a, b, c and d represented as such:

0 0 0 0 0 1 1 0 <- a
0 1 1 0 0 1 1 0 <- b
0 0 0 0 0 0 0 0 <- c
1 1 1 1 1 1 1 1 <- d
To have a == b means that every 0 and 1 match on both variables. Meaning that a XOR (operator ^) would evaluate to 0 ((a ^ b) == 0), as it excludes by definition any equalities.Now, if a != b, this means that there’s at least somewhere a 1 and a 0 not matching between a and b, making (a ^ b) != 0.Both formulas are logically equivalent and using the XOR bitwise operator costs actually the same amount of gas.However, it is much cheaper to use the bitwise OR operator (|) than comparing the truthy or falsy values.These are logically equivalent too, as the OR bitwise operator (|) would result in a 1 somewhere if any value is not 0 between the XOR (^) statements, meaning if any XOR (^) statement verifies that its arguments are different.

```solidity

file: PrizeVault.sol

458  if (_totalAssets == 0)

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L458

```solidity

file: abstract/Claimable.sol

65  if (address(prizePool_) == address(0)) revert PrizePoolZeroAddress();
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L65

## [G-19] Using this to access functions results in an external call, wasting gas
External calls have an overhead of 100 gas, which can be avoided by not referencing the function using this. Contracts are allowed to override their parents' functions and change the visibility from external to public, so make this change if it's required in order to call the function internally.

```solidity

file: PrizeVault.sol

337    return yieldVault.convertToAssets(yieldVault.balanceOf(address(this))) + _asset.balanceOf(address(this));

382  uint256 _latentBalance = _asset.balanceOf(address(this));
383   uint256 _maxYieldVaultDeposit = yieldVault.maxDeposit(address(this));

405    uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

416  uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

539   if (_asset.allowance(_owner, address(this)) != _assets) {
540    IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s

558   if (twabController.delegateOf(address(this), _owner) != SPONSORSHIP_ADDRESS) 

634   if (_tokenOut == address(this))

639 _maxAmountOut = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

691  else if (_tokenOut == address(this)) 

713    prizePool.contributePrizeTokens(address(this), _amountIn);

726  return (_tokenOut == address(_asset) || _tokenOut == address(this)) && _liquidationPair == liquidationPair;  

747  emit LiquidationPairSet(address(this), address(_liquidationPair));

856     address(this),

861   uint256 _assetsWithDust = _asset.balanceOf(addres

866  uint256 _assetsUsed = yieldVault.mint(_yieldVaultShares, address(this));

922   return yieldVault.convertToAssets(yieldVault.maxRedeem(address(this)));

931  uint256 _latentAssets = _asset.balanceOf(address(this));

936  yieldVault.redeem(_yieldVaultShares, address(this), address(this));

938  if (_receiver != address(this))
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L337
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L382-L383
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L405
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L416
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L539-L540
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L558
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L634
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L639
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L691
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L713
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L726
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L747
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L856
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L861
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L866
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L922
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L931
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L936
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L938

```solidity

file: TwabERC20.sol

59  return twabController.balanceOf(address(this), _account);

64  return twabController.totalSupply(address(this));
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L59
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L64