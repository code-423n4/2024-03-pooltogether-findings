
## Low Findings

|    | Issue | Instances |
|----|-------|:---------:|
| [L-01] | Centralization risk for privileged functions | 6 |
| [L-02] | Subtraction in `unchecked` block is unsafe | 1 |
| [L-03] | `decimals()` is not part of the ERC20 standard | 1 |
| [L-04] | Some tokens may revert on large transfers | 2 |
| [L-05] | Code does not follow the best practice of check-effects-interaction | 1 |
| [L-06] | Use `abi.encodeCall()` instead of `abi.encodeWithSignature()`/`abi.encodeWithSelector()` | 1 |
| [L-07] | Upgradable contracts not taken into account | 3 |
| [L-08] | Lack of index element validation in external/public function | 5 |
| [L-09] | Consider checking that transfer to `address(this)` is disabled | 3 |
| [L-10] | Unchecked Return Values of the `approve()` Function | 2 |
| [L-11] | Use `forceApprove/safeIncreaseAllowance` from SafeERC20 instead of `approve/safeApprove` | 1 |
| [L-12] | Unsafe use of `transfer()/transferFrom()` with IERC20 | 2 |
| [L-13] | Unchecked Return Values of `transfer()/transferFrom()` | 2 |
| [L-14] | Solidity version 0.8.20 may not work on other chains due to `PUSH0` | 6 |
| [L-15] | Missing checks for `address(0x0)` when updating address state variables | 1 |
| [L-16] | Lack of Parameter Validation in Constructor/Initializer | 1 |
| [L-17] | Owner can renounce Ownership | 1 |
| [L-18] | Missing Contract-Existence Checks Before Low-Level Calls | 1 |
| [L-19] | Events may be emitted out of order due to reentrancy | 3 |
| [L-20] | Critical functions should be a two step procedure | 5 |
| [L-21] | Array is `push()`ed but not `pop()`ed | 1 |
| [L-22] | Consider implementing two-step procedure for updating protocol addresses | 4 |
| [L-23] | Function Parameters in Public Accessible Functions Need `address(0)` Check | 1 |
| [L-24] | Missing `address(0)` Check in Constructor | 2 |
| [L-25] | Functions calling tokens with transfer hooks are missing reentrancy guards | 4 |
| [L-26] | Consider Using `Ownable2Step` rather than `Ownable` | 2 |

Total: 62 instances of 26 issues

## NonCritical Findings

|    | Issue | Instances |
|----|-------|:---------:|
| [N-01] | Loss of precision | 1 |
| [N-02] | Non-library/interface files should use fixed compiler versions, not floating ones | 5 |
| [N-03] | Consider splitting long calculations | 1 |
| [N-04] | Multiple `address`/ID mappings can be combined into a single `mapping` of an `address`/ID to a `struct`, for readability | 2 |
| [N-05] | Consider using a `struct` rather than having many function input parameters | 5 |
| [N-06] | Consider using descriptive `constant`s when passing zero as a function argument | 1 |
| [N-07] | Custom error has no error details | 14 |
| [N-08] | Consider using OpenZeppelin's SafeCast for any casting | 1 |
| [N-09] | Consider emitting an event at the end of the constructor | 3 |
| [N-10] | Consider providing a ranged getter for array state variables | 1 |
| [N-11] | Consider using named returns | 39 |
| [N-12] | Consider using named function arguments | 4 |
| [N-13] | Events are missing sender information | 3 |
| [N-14] | Leverage Recent Solidity Features with `0.8.23` | 1 |
| [N-15] | NatSpec: Function `@param` tag is missing | 1 |
| [N-16] | High cyclomatic complexity | 1 |
| [N-17] | Contract uses both `require()`/`revert()` as well as custom errors | 2 |
| [N-18] | Solidity Version Too Recent to be Trusted | 5 |
| [N-19] | Inconsistent spacing in comments | 140 |
| [N-20] | Unused `error` definition | 1 |
| [N-21] | Critical system parameter changes should be behind a timelock | 4 |
| [N-22] | Outdated Solidity Version | 1 |
| [N-23] | Ensure Non-Empty Check for Bytes in Function Parameters | 2 |
| [N-24] | Function/Constructor Argument Names Not in mixedCase | 44 |
| [N-25] | Upgrade `openzeppelin` to the Latest Version - 5.0.0 | 8 |
| [N-26] | Duplicated `require()`/`revert()` checks should be refactored to a modifier or function | 2 |
| [N-27] | Event is not properly `indexed` | 4 |
| [N-28] | Consider Using `safePermit()` Instead of `permit()` | 1 |
| [N-29] | Consider Adding Emergency-Stop Functionality | 2 |
| [N-30] | Consider adding a block/deny-list | 2 |
| [N-31] | Non-uppercase/Constant-case Naming for `immutable` Variables | 6 |
| [N-32] | Insufficient Comments in Complex Functions | 1 |
| [N-33] | `constant`s should be defined rather than using magic numbers | 2 |
| [N-34] | Consider moving `msg.sender` checks to a common authorization `modifier` | 1 |
| [N-35] | NatSpec: Contract declarations should have `@author` tag | 1 |
| [N-36] | Consider defining system-wide constants in a single file | 4 |
| [N-37] | Add inline comments for unnamed variables | 4 |
| [N-38] | Consider making contracts `Upgradeable` | 5 |
| [N-39] | Events that mark critical parameter changes should contain both the old and the new value | 2 |
| [N-40] | Style guide: Contract does not follow the Solidity style guide's suggested layout ordering | 2 |
| [N-41] | Style guide: Control structures do not follow the Solidity Style Guide | 18 |
| [N-42] | Constants in comparisons should appear on the left side | 10 |
| [N-43] | NatSpec: Contract declarations should have `@notice` tag | 1 |
| [N-44] | Long functions should be refactored into multiple, smaller, functions | 1 |
| [N-45] | NatSpec: Contract declarations should have `@dev` tags | 2 |
| [N-46] | `public` functions not called by the contract should be declared `external` instead | 6 |
| [N-47] | `else`-block not required | 7 |
| [N-48] | Style guide: Function ordering does not follow the Solidity style guide | 8 |
| [N-49] | Style guide: Extraneous whitespace | 47 |
| [N-50] | Unused import | 1 |
| [N-51] | `if`-statement can be converted to a ternary | 1 |
| [N-52] | Consider using named mappings | 1 |
| [N-53] | Use of `override` is unnecessary | 6 |
| [N-54] | Setters should prevent re-setting of the same value | 7 |
| [N-55] | NatSpec: Contract declarations should have `@title` tags | 1 |
| [N-56] | Contracts should have full test coverage | 1 |
| [N-57] | Large or complicated code bases should implement invariant tests | 1 |
| [N-58] | Implement Formal Verification Proofs to Improve Security | 1 |

Total: 449 instances of 58 issues

## Low Findings Details

### [L-01] Centralization risk for privileged functions

Functions that grant centralized privileges introduce a notable security risk.
Such functions can act as a single point of vulnerability, especially if they control critical operations or access to sensitive data.
        
If the account or entity with these privileges gets compromised, it could lead to the unauthorized alteration of the contract's state, asset misappropriation, or other malicious activities.
It's essential to implement robust access controls, frequent audits, and potentially decentralize critical functions to mitigate this risk.

<details>
<summary><i>6 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

703: function verifyTokensIn(
        address _tokenIn,
        uint256 _amountIn,
        bytes calldata
    ) external onlyLiquidationPair
735: function setClaimer(address _claimer) external onlyOwner
742: function setLiquidationPair(address _liquidationPair) external onlyOwner
753: function setYieldFeePercentage(uint32 _yieldFeePercentage) external onlyOwner
759: function setYieldFeeRecipient(address _yieldFeeRecipient) external onlyOwner
```
[703](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L703) | [735](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L735) | [742](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L742) | [753](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L753) | [759](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L759)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

76: function claimPrize(
        address _winner,
        uint8 _tier,
        uint32 _prizeIndex,
        uint96 _reward,
        address _rewardRecipient
    ) external onlyClaimer returns (uint256)
```
[76](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L76)
</details>

### [L-02] Subtraction in `unchecked` block is unsafe

Performing subtraction within an unchecked block poses a risk of silent underflow, leading to unintended behavior or vulnerabilities. Without proper value checks, the subtraction could result in values that are lower than expected, potentially impacting the logic and security of the contract.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

800: return type(uint96).max - _totalSupply;
```
[800](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L800)
</details>

### [L-03] `decimals()` is not part of the ERC20 standard

It's often used, but `decimals()` is not part of the original ERC-20 standard. It was added later as an optional extension. 
Some valid ERC-20 tokens do not support this function, so casting all tokens to an interface that includes `decimals()` can be unsafe.
Consider revising your code to account for tokens that may not implement the `decimals()` function.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

774: abi.encodeWithSelector(IERC20Metadata.decimals.selector)
```
[774](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L774)
</details>

### [L-04] Some tokens may revert on large transfers

Tokens like:
            
- [COMP (Compound Protocol)](https://github.com/compound-finance/compound-protocol/blob/a3214f67b73310d547e00fc578e8355911c9d376/contracts/Governance/Comp.sol#L115-L142)
- [UNI (Uniswap)](https://github.com/Uniswap/governance/blob/eabd8c71ad01f61fb54ed6945162021ee419998e/contracts/Uni.sol#L209-L236)
            
have limit set by `uint96`.
            
Transfers that approach or exceed this limit will revert.
Be cautious of such transfers, especially if batching is not implemented to handle these scenarios.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

854: _asset.safeTransferFrom(
            _caller,
            address(this),
            _assets
        )
939: _asset.transfer(_receiver, _assets)
```
[854](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L854) | [939](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L939)
</details>

### [L-05] Code does not follow the best practice of check-effects-interaction

Code should follow the best-practice of [check-effects-interaction](https://blockchain-academy.hs-mittweida.de/courses/solidity-coding-beginners-to-intermediate/lessons/solidity-11-coding-patterns/topic/checks-effects-interactions/), where state variables are updated before any external calls are made. Doing so prevents a large class of reentrancy bugs.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

/// @audit - State change after external call: `IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER)`
121: deployedVaults[address(_vault)] = true
```
[121](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L121)
</details>

### [L-06] Use `abi.encodeCall()` instead of `abi.encodeWithSignature()`/`abi.encodeWithSelector()`

As of Solidity version 0.8.11, `abi.encodeCall()` provides type-safe encoding utility, a notable enhancement over `abi.encodeWithSelector()`.
While the latter can prevent typographical errors, it doesn't provide type checking.
The use of `abi.encodeCall()` introduces type safety at compile time, leading to safer and more robust smart contracts.
[Read more about type safety](https://github.com/OpenZeppelin/openzeppelin-contracts/issues/3693)

Please note that adopting `abi.encodeCall()` requires bumping the Solidity pragma, which could introduce breaking changes.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

774: abi.encodeWithSelector(IERC20Metadata.decimals.selector)
```
[774](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L774)
</details>

### [L-07] Upgradable contracts not taken into account

In the realm of blockchain development, it's crucial to consider the impact of upgradable contracts, especially when handling token addresses through interfaces like IERC20.
These contracts can evolve over time, potentially altering their behavior or interface. Such changes may lead to compatibility issues or security vulnerabilities in the protocol that relies on them.

<details>
<summary><i>3 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

303: IERC20 asset_ = IERC20(yieldVault_.asset());
540: IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);
```
[303](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L303) | [540](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L540)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

118: IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER);
```
[118](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L118)
</details>

### [L-08] Lack of index element validation in external/public function

There's no validation to check whether the index element provided as an argument actually exists in the call. This omission could lead to unintended behavior if an element that does not exist in the call is passed to the function.

The function should validate that the provided index element exists in the call before proceeding.

Without this validation, the function could cause unintended behaviour as it will call an non-existing index element. This could lead to inconsistencies in data and potentially affect the integrity of the call structure.

<details>
<summary><i>5 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

85: _hooks[_winner]
86: _hooks[_winner]
108: _hooks[_winner]
109: _hooks[_winner]
```
[85](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L85) | [86](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L86) | [108](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L108) | [109](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L109)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

23: _hooks[account]
```
[23](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L23)
</details>

### [L-09] Consider checking that transfer to `address(this)` is disabled

Functions allowing users to specify the receiving address should check whether the referenced address is not `address(this)`.

This would prevent tokens to remain lock inside the contract due to an incorrect `transfer` or `mint`.

<details>
<summary><i>3 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

692: _mint(_receiver, _amountOut)
872: _mint(_receiver, _shares)
939: _asset.transfer(_receiver, _assets)
```
[692](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L692) | [872](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L872) | [939](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L939)
</details>

### [L-10] Unchecked Return Values of the `approve()` Function

The unchecked return value of the `approve()` method can potentially cause transaction failures to go unnoticed in your contract. 

Some IERC20 token implementations utilize boolean return values to indicate transaction failures, instead of relying on the `revert()` function. If the return value of the `approve()` method isn't appropriately verified, transactions may seemingly proceed even when the necessary token approvals have not been appropriately executed.

As a safeguard, it is crucial to consistently verify the return values of these methods. This precaution helps to ensure the correct execution of token approvals, maintaining the integrity of transaction operations and reducing the risk of unnoticed transaction failures.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

862: _asset.approve(address(yieldVault), _assetsWithDust);
869: _asset.approve(address(yieldVault), 0);
```
[862](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L862) | [869](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L869)
</details>

### [L-11] Use `forceApprove/safeIncreaseAllowance` from SafeERC20 instead of `approve/safeApprove`

Certain tokens, exemplified by USDT, have quirks where adjusting an allowance directly from a non-zero value can be problematic. For such tokens, it's a common requirement to first reset the allowance to zero before setting a new value. Failing to do so can result in transaction reverts, disrupting protocol functionality.

To gracefully handle this, consider using the `forceApprove` method from OpenZeppelin's SafeERC20 library. This method ensures the contract's allowance towards a `spender` is set to the specified `value`. If the token doesn't return a value, the non-reverting calls are assumed successful, offering a seamless solution for tokens like USDT.

Since `safeIncreaseAllowance` calls `forceApprove` internally, it's also a viable alternative.

[Reference the SafeERC20 Library for more details](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/9e3f4d60c581010c4a3979480e07cc7752f124cc/contracts/token/ERC20/utils/SafeERC20.sol#L71C8-L83).

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

862: _asset.approve(address(yieldVault), _assetsWithDust);
```
[862](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L862)
</details>

### [L-12] Unsafe use of `transfer()/transferFrom()` with IERC20

Not all tokens adhere strictly to the ERC20 standard, even if they are largely accepted as compliant.
A prominent example is Tether (USDT) on L1, where the transfer() and transferFrom() functions do not return booleans, contrary to the standard's requirements.
Instead, these functions provide no return value at all.
This discrepancy can lead to issues when such tokens are cast to IERC20: their non-standard function signatures can cause calls to revert.
As a safer alternative, consider using OpenZeppelin's SafeERC20 library, which offers safeTransfer() and safeTransferFrom() functions to address these incompatibilities.

<details>
<summary><i>2 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

939: _asset.transfer(_receiver, _assets);
```
[939](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L939)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

118: IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER);
```
[118](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L118)
</details>

### [L-13] Unchecked Return Values of `transfer()/transferFrom()`

`transfer()` or `transferFrom()` methods without checking their return values.
Some IERC20 implementations use these boolean return values to signal transaction failures instead of using revert().
Not checking these values could allow transactions to proceed without the intended token transfers.
It is recommended to always check the return values of these methods.

<details>
<summary><i>2 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

939: _asset.transfer(_receiver, _assets);
```
[939](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L939)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

118: IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER);
```
[118](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L118)
</details>

### [L-14] Solidity version 0.8.20 may not work on other chains due to `PUSH0`

Solidity 0.8.20, by default, targets the Shanghai version of the EVM, which includes the new `PUSH0` opcode.
However, this opcode may not yet be implemented on all Layer-2 chains.
For instance Arbitrum does not yet support `PUSH0`. See [Documentation](https://developer.arbitrum.io/solidity-support) for more information.
As such, deploying contracts compiled with Solidity 0.8.20 on these chains could lead to failures.

To avoid this, consider specifying an older EVM version as the target, or ensure that the Layer-2 chain used supports the Shanghai EVM.

<details>
<summary><i>6 issue instances in 6 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

2: pragma solidity ^0.8.24;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L2)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

2: pragma solidity ^0.8.24;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L2)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

2: pragma solidity ^0.8.24;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L2)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

2: pragma solidity ^0.8.24;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L2)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

2: pragma solidity ^0.8.0;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L2)

```solidity
File: pt-v5-vault/src/interfaces/IVaultHooks.sol

2: pragma solidity ^0.8.24;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol#L2)
</details>

### [L-15] Missing checks for `address(0x0)` when updating address state variables

State variables that are of type `address` should always be checked to ensure that they are not being assigned the null address (address(0x0)).
A null address often implies an error or omission in the code.
Without proper checks, such a scenario can lead to unexpected behavior and potential vulnerabilities in the smart contract.
Therefore, it is considered good practice to implement checks for `address(0x0)` prior to assigning new values to address state variables.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

959: yieldFeeRecipient = _yieldFeeRecipient;
```
[959](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L959)
</details>

### [L-16] Lack of Parameter Validation in Constructor/Initializer

The constructor/initializer doesn't validate input parameters before assigning them to state variables.
It is crucial to ensure that input parameters meet certain conditions to avoid unexpected states or behaviors in the contract.
Consider adding appropriate checks or constraints.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit `yieldFeePercentage_` is not validated before use
/// @audit `yieldBuffer_` is not validated before use
289: constructor(
        string memory name_,
        string memory symbol_,
        IERC4626 yieldVault_,
        PrizePool prizePool_,
        address claimer_,
        address yieldFeeRecipient_,
        uint32 yieldFeePercentage_,
        uint256 yieldBuffer_,
        address owner_
    ) TwabERC20(name_, symbol_, prizePool_.twabController()) Claimable(prizePool_, claimer_) Ownable(owner_) {
```
[289](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L289)
</details>

### [L-17] Owner can renounce Ownership

The contract uses solady/OpenZeppelin's `Ownable(Upgradable).sol` and contains `onlyOwner` functions.
Consider implementing custom logic for `renounceOwnership()` to handle renouncing ownership.
`onlyOwner` functions:

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

8: import { Ownable } from "owner-manager-contracts/Ownable.sol";
```
[8](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L8)
</details>

### [L-18] Missing Contract-Existence Checks Before Low-Level Calls

When making low-level calls, it's crucial to ensure the existence of the contract at the specified address. 
If the contract doesn't exist at the given address, low-level calls will still return success, potentially causing errors in the code execution.
Therefore, alongside zero-address checks, adding an additional check to verify that <address>.code.length > 0 before making low-level calls would be recommended.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

773: (bool success, bytes memory encodedDecimals) = address(asset_).staticcall(
```
[773](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L773)
</details>

### [L-19] Events may be emitted out of order due to reentrancy

It's essential to ensure that events follow the best practice of check-effects-interaction and are emitted before any external calls to prevent out-of-order events due to reentrancy.
Emitting events post external interactions may cause them to be out of order due to reentrancy, which can be misleading or erroneous for event listeners.
[Refer to the Solidity Documentation for best practices.](https://solidity.readthedocs.io/en/latest/security-considerations.html#reentrancy)

<details>
<summary><i>3 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit `safeTransferFrom()` before `Deposit` event emit
875: emit Deposit(_caller, _receiver, _assets, _shares);
```
[875](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L875)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

/// @audit `transferFrom()` before `NewPrizeVault` event emit
122: emit NewPrizeVault(
            _vault,
            _yieldVault,
            _prizePool,
            _name,
            _symbol
        );
```
[122](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L122)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

/// @audit `transfer()` before `Transfer` event emit
102: emit Transfer(_from, _to, _amount);
```
[102](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L102)
</details>

### [L-20] Critical functions should be a two step procedure

Critical functions in Solidity contracts should follow a two-step procedure to enhance security, minimize human error, and ensure proper access control. By dividing sensitive operations into distinct phases, such as initiation and confirmation, developers can introduce a safeguard against unintended actions or unauthorized access.

<details>
<summary><i>5 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

735: function setClaimer(address _claimer) external onlyOwner {
742: function setLiquidationPair(address _liquidationPair) external onlyOwner {
753: function setYieldFeePercentage(uint32 _yieldFeePercentage) external onlyOwner {
759: function setYieldFeeRecipient(address _yieldFeeRecipient) external onlyOwner {
```
[735](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L735) | [742](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L742) | [753](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L753) | [759](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L759)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

29: function setHooks(VaultHooks calldata hooks) external {
```
[29](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L29)
</details>

### [L-21] Array is `push()`ed but not `pop()`ed

Arrays in the contract have entries added using the `push` method, but no corresponding `pop` method is found to remove them. 
Ensure to consider whether old entries need to be removed or if there should be a maximum array length.
Cases with specific potential issues might be reported separately under different issue categories.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

120: allVaults.push(_vault);
```
[120](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L120)
</details>

### [L-22] Consider implementing two-step procedure for updating protocol addresses

Direct state address changes in a function can be risky, as they don't allow for a verification step before the change is made.
It's safer to implement a two-step process where the new address is first proposed, then later confirmed, allowing for more control and the chance to catch errors or malicious activity.

<details>
<summary><i>4 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

736: _setClaimer(_claimer);
744: liquidationPair = _liquidationPair;
760: _setYieldFeeRecipient(_yieldFeeRecipient);
959: yieldFeeRecipient = _yieldFeeRecipient;
```
[736](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L736) | [744](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L744) | [760](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L760) | [959](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L959)
</details>

### [L-23] Function Parameters in Public Accessible Functions Need `address(0)` Check

Parameters of type `address` in your functions should be checked to ensure that they are not assigned the null address (`address(0x0)`). 
Failure to validate these parameters can lead to transaction reverts, wasted gas, the need for transaction resubmission, and may even require redeployment of contracts within the protocol in certain situations.
Implement checks for `address(0x0)` to avoid these potential issues.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

/// @audit `_claimer` parameter without address(0) check
/// @audit `_yieldFeeRecipient` parameter without address(0) check
/// @audit `_owner` parameter without address(0) check
92: function deployVault(
      string memory _name,
      string memory _symbol,
      IERC4626 _yieldVault,
      PrizePool _prizePool,
      address _claimer,
      address _yieldFeeRecipient,
      uint32 _yieldFeePercentage,
      address _owner
    ) external returns (PrizeVault) {
```
[92](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L92)
</details>

### [L-24] Missing `address(0)` Check in Constructor

The constructor does not include a check for `address(0)` when set state variables that hold addresses.
Initializing a state variable with `address(0)` can lead to unintended behavior and vulnerabilities in the contract, such as sending funds to an inaccessible address. 
It is recommended to include a validation step to ensure that address parameters are not set to `address(0)`.

<details>
<summary><i>2 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit `yieldFeeRecipient_` has lack of `address(0)` check before use
289: constructor(
        string memory name_,
        string memory symbol_,
        IERC4626 yieldVault_,
        PrizePool prizePool_,
        address claimer_,
        address yieldFeeRecipient_,
        uint32 yieldFeePercentage_,
        uint256 yieldBuffer_,
        address owner_
    ) TwabERC20(name_, symbol_, prizePool_.twabController()) Claimable(prizePool_, claimer_) Ownable(owner_) {
```
[289](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L289)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

/// @audit `claimer_` has lack of `address(0)` check before use
64: constructor(PrizePool prizePool_, address claimer_) {
```
[64](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L64)
</details>

### [L-25] Functions calling tokens with transfer hooks are missing reentrancy guards

Functions that call contracts or addresses with transfer hooks should use reentrancy guards for protection. 
Even if these functions adhere to the check-effects-interaction best practice, absence of reentrancy guards can expose the protocol users to read-only reentrancies.
Without the guards, the only protective measure is to block-list the entire protocol, which isn't an optimal solution.

<details>
<summary><i>4 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit function `_tryGetAssetDecimals()` 
773: (bool success, bytes memory encodedDecimals) = address(asset_).staticcall(
            abi.encodeWithSelector(IERC20Metadata.decimals.selector)
        );
/// @audit function `_depositAndMint()` 
854: _asset.safeTransferFrom(
            _caller,
            address(this),
            _assets
        );
/// @audit function `_withdraw()` 
939: _asset.transfer(_receiver, _assets);
```
[773](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L773) | [854](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L854) | [939](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L939)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

/// @audit function `deployVault()` 
118: IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER);
```
[118](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L118)
</details>

### [L-26] Consider Using `Ownable2Step` rather than `Ownable`

To enhance the security and prevent inadvertent ownership transfers, it's advisable to use `Ownable2Step` or `Ownable2StepUpgradeable`.
Contracts necessitate an active confirmation from the recipient before the ownership transfer is finalized.
This mechanism serves as a safeguard against scenarios where, for instance, a typo in the address could lead to unintentional ownership changes.

By implementing a two-step confirmation process, contracts can better ensure the accurate and intentional transfer of ownership.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

8: import { Ownable } from "owner-manager-contracts/Ownable.sol";
65: contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownable {
```
[8](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L8) | [65](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L65)
</details>


## NonCritical Findings Details

### [N-01] Loss of precision

Division by large numbers may result in the result being zero, due to Solidity not supporting fractions. Consider requiring a minimum amount for the numerator to ensure that it is always larger than the denominator.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

675: _yieldFee = (_amountOut * FEE_PRECISION) / (FEE_PRECISION - _yieldFeePercentage) - _amountOut;
```
[675](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L675)
</details>

### [N-02] Non-library/interface files should use fixed compiler versions, not floating ones

Floating pragmas may lead to unintended vulnerabilities due to different compiler versions.
It is recommended to lock the Solidity version in pragma statements.

<details>
<summary><i>5 issue instances in 5 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

2: pragma solidity ^0.8.24
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L2)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

2: pragma solidity ^0.8.24
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L2)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

2: pragma solidity ^0.8.24
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L2)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

2: pragma solidity ^0.8.24
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L2)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

2: pragma solidity ^0.8.0
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L2)
</details>

### [N-03] Consider splitting long calculations

For improved readability and maintainability, it's suggested to limit arithmetic operations to 3 per expression.
Excessive operations can convolute the code, making it more prone to errors and more difficult to debug or review.
Splitting operations across multiple lines is often a good approach.
Special care should be taken with division operations. The number of arithmetic operations per line ideally shouldn't exceed three.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

675: (_amountOut * FEE_PRECISION) / (FEE_PRECISION - _yieldFeePercentage) - _amountOut
```
[675](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L675)
</details>

### [N-04] Multiple `address`/ID mappings can be combined into a single `mapping` of an `address`/ID to a `struct`, for readability

Combining multiple address/ID mappings into a single mapping to a struct can enhance code clarity and maintainability. 
Consider refactoring multiple mappings into a single mapping with a struct for cleaner code structure.
This arrangement also promotes a more organized contract structure, making it easier for developers to navigate and understand.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

69: mapping(address vault => bool deployedByFactory) public deployedVaults
72: mapping(address deployer => uint256 nonce) public deployerNonces
```
[69](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L69) | [72](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L72)
</details>

### [N-05] Consider using a `struct` rather than having many function input parameters

Functions with many parameters can become difficult to read and maintain. 
Using a struct to encapsulate these parameters can improve code readability, increase reusability, and reduce the likelihood of errors.
Consider refactoring functions that take more than three parameters to use a struct instead.

<details>
<summary><i>5 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

289: constructor(
        string memory name_,
        string memory symbol_,
        IERC4626 yieldVault_,
        PrizePool prizePool_,
        address claimer_,
        address yieldFeeRecipient_,
        uint32 yieldFeePercentage_,
        uint256 yieldBuffer_,
        address owner_
    ) TwabERC20(name_, symbol_, prizePool_.twabController()) Claimable(prizePool_, claimer_) Ownable(owner_)
524: function depositWithPermit(
        uint256 _assets,
        address _owner,
        uint256 _deadline,
        uint8 _v,
        bytes32 _r,
        bytes32 _s
    ) external returns (uint256)
887: function _burnAndWithdraw(
        address _caller,
        address _receiver,
        address _owner,
        uint256 _shares,
        uint256 _assets
    ) internal
```
[289](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L289) | [524](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L524) | [887](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L887)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

92: function deployVault(
      string memory _name,
      string memory _symbol,
      IERC4626 _yieldVault,
      PrizePool _prizePool,
      address _claimer,
      address _yieldFeeRecipient,
      uint32 _yieldFeePercentage,
      address _owner
    ) external returns (PrizeVault)
```
[92](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L92)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

76: function claimPrize(
        address _winner,
        uint8 _tier,
        uint32 _prizeIndex,
        uint96 _reward,
        address _rewardRecipient
    ) external onlyClaimer returns (uint256)
```
[76](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L76)
</details>

### [N-06] Consider using descriptive `constant`s when passing zero as a function argument

In instances where utilizing a zero parameter is essential, it is recommended to employ descriptive constants or an enum instead of directly integrating zero within function calls.
This strategy aids in clearly articulating the caller's intention and minimizes the risk of errors.
Emitting zero also not recomended, as it is not clear what the intention is.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

869: _asset.approve(address(yieldVault), 0)
```
[869](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L869)
</details>

### [N-07] Custom error has no error details

Take advantage of custom error's return value property. 

An important feature of Custom Error is that values such as address, tokenID, msg.value can be written inside the () sign, providing a serious advantage in debugging and examining the revert details of dapps such as Tenderly.

<details>
<summary><i>14 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

182: error YieldVaultZeroAddress()
185: error OwnerZeroAddress()
188: error WithdrawZeroAssets()
191: error BurnZeroShares()
194: error DepositZeroAssets()
197: error MintZeroShares()
200: error ZeroTotalAssets()
203: error LPZeroAddress()
206: error SweepZeroAssets()
209: error LiquidationAmountOutZero()
```
[182](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L182) | [185](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L185) | [188](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L188) | [191](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L191) | [194](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L194) | [197](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L197) | [200](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L200) | [203](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L203) | [206](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L206) | [209](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L209)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

33: error TwabControllerZeroAddress()
```
[33](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L33)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

34: error PrizePoolZeroAddress()
37: error ClaimerZeroAddress()
40: error ClaimRecipientZeroAddress()
```
[34](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L34) | [37](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L37) | [40](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L40)
</details>

### [N-08] Consider using OpenZeppelin's SafeCast for any casting

OpenZeppelin's has `SafeCast` library that provides functions to safely cast. Recommended to use it instead of casting directly in any case.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

779: uint8(returnedDecimals)
```
[779](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L779)
</details>

### [N-09] Consider emitting an event at the end of the constructor

This will allow users to easily exactly pinpoint when and by whom a contract was constructed.

<details>
<summary><i>3 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

289: constructor(
        string memory name_,
        string memory symbol_,
        IERC4626 yieldVault_,
        PrizePool prizePool_,
        address claimer_,
        address yieldFeeRecipient_,
        uint32 yieldFeePercentage_,
        uint256 yieldBuffer_,
        address owner_
    ) TwabERC20(name_, symbol_, prizePool_.twabController()) Claimable(prizePool_, claimer_) Ownable(owner_)
```
[289](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L289)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

42: constructor(
        string memory name_,
        string memory symbol_,
        TwabController twabController_
    ) ERC20(name_, symbol_) ERC20Permit(name_)
```
[42](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L42)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

64: constructor(PrizePool prizePool_, address claimer_)
```
[64](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L64)
</details>

### [N-10] Consider providing a ranged getter for array state variables

While the compiler automatically provides a getter for accessing single elements within a public state variable array, it doesn't provide a way to fetch the whole array, or subsets thereof.
Consider adding a function to allow the fetching of slices of the array, especially if the contract doesn't already have multicall functionality.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

66: PrizeVault[] public allVaults
```
[66](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L66)
</details>

### [N-11] Consider using named returns

Using named returns makes the code more self-documenting, makes it easier to fill out NatSpec, and in some cases can save gas.
The cases below are where there currently is at most one return statement, which is ideal for named returns.

<details>
<summary><i>39 issue instances in 5 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

320: function decimals() public view override(ERC20, IERC20Metadata) returns (uint8)
329: function asset() external view returns (address)
336: function totalAssets() public view returns (uint256)
341: function convertToShares(uint256 _assets) public view returns (uint256)
355: function convertToAssets(uint256 _shares) public view returns (uint256)
374: function maxDeposit(address) public view returns (uint256)
397: function maxMint(address _owner) public view returns (uint256)
404: function maxWithdraw(address _owner) public view returns (uint256)
415: function maxRedeem(address _owner) public view returns (uint256)
441: function previewDeposit(uint256 _assets) public pure returns (uint256)
447: function previewMint(uint256 _shares) public pure returns (uint256)
454: function previewWithdraw(uint256 _assets) public view returns (uint256)
470: function previewRedeem(uint256 _shares) public view returns (uint256)
475: function deposit(uint256 _assets, address _receiver) external returns (uint256)
482: function mint(uint256 _shares, address _receiver) external returns (uint256)
489: function withdraw(
        uint256 _assets,
        address _receiver,
        address _owner
    ) external returns (uint256)
500: function redeem(
        uint256 _shares,
        address _receiver,
        address _owner
    ) external returns (uint256)
524: function depositWithPermit(
        uint256 _assets,
        address _owner,
        uint256 _deadline,
        uint8 _v,
        bytes32 _r,
        bytes32 _s
    ) external returns (uint256)
552: function sponsor(uint256 _assets) external returns (uint256)
573: function totalDebt() public view returns (uint256)
584: function totalYieldBalance() public view returns (uint256)
591: function availableYieldBalance() public view returns (uint256)
597: function currentYieldBuffer() external view returns (uint256)
631: function liquidatableBalanceOf(address _tokenOut) public view returns (uint256)
659: function transferTokensOut(
        address,
        address _receiver,
        address _tokenOut,
        uint256 _amountOut
    ) public virtual onlyLiquidationPair returns (bytes memory)
717: function targetOf(address) external view returns (address)
722: function isLiquidationPair(
        address _tokenOut,
        address _liquidationPair
    ) external view returns (bool)
772: function _tryGetAssetDecimals(IERC20 asset_) internal view returns (bool, uint8)
790: function _totalDebt(uint256 _totalSupply) internal view returns (uint256)
798: function _twabSupplyLimit(uint256 _totalSupply) internal pure returns (uint256)
808: function _totalYieldBalance(uint256 _totalAssets, uint256 totalDebt_) internal pure returns (uint256)
823: function _availableYieldBalance(uint256 _totalAssets, uint256 totalDebt_) internal view returns (uint256)
921: function _maxYieldVaultWithdraw() internal view returns (uint256)
```
[320](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L320) | [329](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L329) | [336](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L336) | [341](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L341) | [355](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L355) | [374](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L374) | [397](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L397) | [404](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L404) | [415](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L415) | [441](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L441) | [447](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L447) | [454](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L454) | [470](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L470) | [475](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L475) | [482](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L482) | [489](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L489) | [500](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L500) | [524](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L524) | [552](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L552) | [573](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L573) | [584](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L584) | [591](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L591) | [597](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L597) | [631](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L631) | [659](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L659) | [717](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L717) | [722](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L722) | [772](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L772) | [790](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L790) | [798](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L798) | [808](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L808) | [823](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L823) | [921](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L921)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

92: function deployVault(
      string memory _name,
      string memory _symbol,
      IERC4626 _yieldVault,
      PrizePool _prizePool,
      address _claimer,
      address _yieldFeeRecipient,
      uint32 _yieldFeePercentage,
      address _owner
    ) external returns (PrizeVault)
136: function totalVaults() external view returns (uint256)
```
[92](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L92) | [136](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L136)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

56: function balanceOf(
        address _account
    ) public view virtual override(ERC20) returns (uint256)
63: function totalSupply() public view virtual override(ERC20) returns (uint256)
```
[56](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L56) | [63](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L63)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

76: function claimPrize(
        address _winner,
        uint8 _tier,
        uint32 _prizeIndex,
        uint96 _reward,
        address _rewardRecipient
    ) external onlyClaimer returns (uint256)
```
[76](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L76)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

22: function getHooks(address account) external view returns (VaultHooks memory)
```
[22](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L22)
</details>

### [N-12] Consider using named function arguments

When calling functions in external contracts with multiple arguments, consider using named function parameters, rather than positional ones.

<details>
<summary><i>4 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

540: IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s)
713: prizePool.contributePrizeTokens(address(this), _amountIn)
936: yieldVault.redeem(_yieldVaultShares, address(this), address(this))
```
[540](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L540) | [713](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L713) | [936](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L936)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

99: prizePool.claimPrize(
            _winner,
            _tier,
            _prizeIndex,
            recipient,
            _reward,
            _rewardRecipient
        )
```
[99](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L99)
</details>

### [N-13] Events are missing sender information

Events should include the sender information when emitted in `public` or `external` functions for better traceability.

<details>
<summary><i>3 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit external function `sponsor()` emits an event without sender information
561: emit Sponsor(_owner, _assets, _shares);
/// @audit external function `setLiquidationPair()` emits an event without sender information
746: emit LiquidationPairSet(address(this), address(_liquidationPair));
```
[561](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L561) | [746](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L746)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

/// @audit external function `deployVault()` emits an event without sender information
122: emit NewPrizeVault(
            _vault,
            _yieldVault,
            _prizePool,
            _name,
            _symbol
        );
```
[122](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L122)
</details>

### [N-14] Leverage Recent Solidity Features with `0.8.23`

The recent updates in Solidity provide several features and optimizations that, when leveraged appropriately, can significantly improve your contract's code clarity and maintainability.
Key enhancements include the use of push0 for placing 0 on the stack for EVM versions starting from "Shanghai", making your code simpler and more straightforward.
Moreover, Solidity has extended NatSpec documentation support to enum and struct definitions, facilitating more comprehensive and insightful code documentation.

Additionally, the re-implementation of the UnusedAssignEliminator and UnusedStoreEliminator in the Solidity optimizer provides the ability to remove unused assignments in deeply nested loops.
This results in a cleaner, more efficient contract code, reducing clutter and potential points of confusion during code review or debugging.
It's recommended to make full use of these features and optimizations to enhance the robustness and readability of your smart contracts.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

2: pragma solidity ^0.8.0;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L2)
</details>

### [N-15] NatSpec: Function `@param` tag is missing

Natural Specification (NatSpec) comments are crucial for understanding the role of function arguments in your Solidity code.
Including `@param` tags will not only improve your code's readability but also its maintainability by clearly defining each argument's purpose.

[More information in Documentation](https://docs.soliditylang.org/en/develop/natspec-format.html)

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/TwabERC20.sol

/// @audit Missed @param `twabController_`
42: constructor(
        string memory name_,
        string memory symbol_,
        TwabController twabController_
    ) ERC20(name_, symbol_) ERC20Permit(name_) {
```
[42](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L42)
</details>

### [N-16] High cyclomatic complexity

Functions with high cyclomatic complexity are harder to understand, test, and maintain.
Consider breaking down these blocks into more manageable units, by splitting things into utility functions,
by reducing nesting, and by using early returns.

[Learn More About Cyclomatic Complexity](https://en.wikipedia.org/wiki/Cyclomatic_complexity)

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit function `transferTokensOut` has a cyclomatic complexity of 7
659: function transferTokensOut(
        address,
        address _receiver,
        address _tokenOut,
        uint256 _amountOut
    ) public virtual onlyLiquidationPair returns (bytes memory) {
```
[659](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L659)
</details>

### [N-17] Contract uses both `require()`/`revert()` as well as custom errors

The contract uses both `require()/revert()` and `custom errors` for error handling.

It's recommended to use a consistent approach for error handling in a single file.

<details>
<summary><i>2 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

65: contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownable {
```
[65](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L65)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

19: contract TwabERC20 is ERC20, ERC20Permit {
```
[19](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L19)
</details>

### [N-18] Solidity Version Too Recent to be Trusted

The current Solidity version used in the code is too recent to be trusted.
Using a newer version might introduce unrecovered bugs. It is recommended to use version 0.8.21.

<details>
<summary><i>5 issue instances in 5 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

2: pragma solidity ^0.8.24;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L2)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

2: pragma solidity ^0.8.24;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L2)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

2: pragma solidity ^0.8.24;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L2)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

2: pragma solidity ^0.8.24;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L2)

```solidity
File: pt-v5-vault/src/interfaces/IVaultHooks.sol

2: pragma solidity ^0.8.24;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol#L2)
</details>

### [N-19] Inconsistent spacing in comments

Some lines use // x and some use //x. The instances below point out the usages that don't follow the majority, within each file:

<details>
<summary><i>140 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

10: /// @title  PoolTogether V5 Prize Vault Factory
11: /// @author PoolTogether Inc. & G9 Software Inc.
12: /// @notice Factory contract for deploying new prize vaults using a standard underlying ERC4626 yield vault.
15: ////////////////////////////////////////////////////////////////////////////////
17: ////////////////////////////////////////////////////////////////////////////////
19: /// @notice Emitted when a new PrizeVault has been deployed by this factory.
20: /// @param vault The vault that was deployed
21: /// @param yieldVault The underlying yield vault
22: /// @param prizePool The prize pool the vault contributes to
23: /// @param name The name of the vault token
24: /// @param symbol The symbol for the vault token
33: ////////////////////////////////////////////////////////////////////////////////
35: ////////////////////////////////////////////////////////////////////////////////
37: /// @notice The yield buffer to use for vault deployments.
38: /// @dev The yield buffer is expected to be of insignificant value and is used to cover rounding
39: /// errors on deposits and withdrawals. Yield is expected to accrue faster than the yield buffer
40: /// can be reasonably depleted.
41: ///
42: /// The yield buffer should be set as high as possible while still being considered
43: /// insignificant for the lowest precision per dollar asset that is expected to be supported.
44: ///
45: /// Precision per dollar (PPD) can be calculated by: (10 ^ DECIMALS) / ($ value of 1 asset).
46: /// For example, USDC has a PPD of (10 ^ 6) / ($1) = 10e6 p/$.
47: ///
48: /// As a rule of thumb, assets with lower PPD than USDC should not be assumed to be compatible since
49: /// the potential loss of a single unit rounding error is likely too high to be made up by yield at
50: /// a reasonable rate. Actual results may vary based on expected gas costs, asset fluctuation, and
51: /// yield accrual rates.
52: ///
53: /// The yield buffer of vaults deployed by this factory is 1e5. This means that if you deploy a
54: /// vault with USDC as the underlying asset, you will have to approve this factory to spend 1e5
55: /// USDC ($0.10) to be sent to the prize vault during deployment. This value will cover the first
56: /// 100k rounding errors on deposits and withdraws to the vault and is not recoverable by the
57: /// deployer.
58: ///
59: /// If the yield buffer is depleted on a vault, the vault will prevent any further
60: /// deposits if it would result in a rounding error and any rounding errors incurred by withdrawals
61: /// will not be covered by yield. The yield buffer will be replenished automatically as yield accrues
62: /// on deposits.
65: /// @notice List of all vaults deployed by this factory.
68: /// @notice Mapping to verify if a Vault has been deployed via this factory.
71: /// @notice Mapping to store deployer nonces for CREATE2
74: ////////////////////////////////////////////////////////////////////////////////
76: ////////////////////////////////////////////////////////////////////////////////
78: /// @notice Deploy a new vault
79: /// @dev Emits a `NewPrizeVault` event with the vault details.
80: /// @dev `claimer` can be set to address zero if none is available yet.
81: /// @dev The caller MUST approve this factory to spend underlying assets equal to `YIELD_BUFFER` so the yield
82: /// buffer can be filled on deployment. This value is unrecoverable and is expected to be insignificant.
83: /// @param _name Name of the ERC20 share minted by the vault
84: /// @param _symbol Symbol of the ERC20 share minted by the vault
85: /// @param _yieldVault Address of the ERC4626 vault in which assets are deposited to generate yield
86: /// @param _prizePool Address of the PrizePool that computes prizes
87: /// @param _claimer Address of the claimer
88: /// @param _yieldFeeRecipient Address of the yield fee recipient
89: /// @param _yieldFeePercentage Yield fee percentage
90: /// @param _owner Address that will gain ownership of this contract
91: /// @return PrizeVault The newly deployed PrizeVault
134: /// @notice Total number of vaults deployed by this factory.
135: /// @return uint256 Number of vaults deployed by this factory.
```
[10](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L10) | [11](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L11) | [12](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L12) | [15](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L15) | [17](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L17) | [19](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L19) | [20](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L20) | [21](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L21) | [22](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L22) | [23](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L23) | [24](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L24) | [33](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L33) | [35](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L35) | [37](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L37) | [38](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L38) | [39](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L39) | [40](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L40) | [41](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L41) | [42](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L42) | [43](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L43) | [44](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L44) | [45](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L45) | [46](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L46) | [47](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L47) | [48](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L48) | [49](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L49) | [50](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L50) | [51](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L51) | [52](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L52) | [53](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L53) | [54](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L54) | [55](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L55) | [56](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L56) | [57](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L57) | [58](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L58) | [59](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L59) | [60](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L60) | [61](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L61) | [62](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L62) | [65](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L65) | [68](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L68) | [71](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L71) | [74](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L74) | [76](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L76) | [78](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L78) | [79](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L79) | [80](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L80) | [81](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L81) | [82](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L82) | [83](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L83) | [84](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L84) | [85](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L85) | [86](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L86) | [87](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L87) | [88](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L88) | [89](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L89) | [90](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L90) | [91](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L91) | [134](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L134) | [135](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L135)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

10: /// @title  PoolTogether V5 TWAB ERC20 Token
11: /// @author G9 Software Inc.
12: /// @notice This contract creates an ERC20 token with balances stored in a TwabController,
13: ///         enabling time-weighted average balances for each depositor and token compatibility
14: ///         with the PoolTogether V5 Prize Pool.
15: /// @dev    This contract is designed to be used as an accounting layer when building a vault
16: ///         for PoolTogether V5.
17: /// @dev    The TwabController limits all balances including total token supply to uint96 for
18: ///         gas savings. Any mints that increase a balance past this limit will fail.
21: ////////////////////////////////////////////////////////////////////////////////
23: ////////////////////////////////////////////////////////////////////////////////
25: /// @notice Address of the TwabController used to keep track of balances.
28: ////////////////////////////////////////////////////////////////////////////////
30: ////////////////////////////////////////////////////////////////////////////////
32: /// @notice Thrown if the TwabController address is the zero address.
35: ////////////////////////////////////////////////////////////////////////////////
37: ////////////////////////////////////////////////////////////////////////////////
39: /// @notice TwabERC20 Constructor
40: /// @param name_ The name of the token
41: /// @param symbol_ The token symbol
51: ////////////////////////////////////////////////////////////////////////////////
53: ////////////////////////////////////////////////////////////////////////////////
55: /// @inheritdoc ERC20
62: /// @inheritdoc ERC20
67: ////////////////////////////////////////////////////////////////////////////////
69: ////////////////////////////////////////////////////////////////////////////////
71: /// @notice Mints tokens to `_receiver` and increases the total supply.
72: /// @dev Emits a {Transfer} event with `from` set to the zero address.
73: /// @dev `_receiver` cannot be the zero address.
74: /// @param _receiver Address that will receive the minted tokens
75: /// @param _amount Tokens to mint
81: /// @notice Destroys tokens from `_owner` and reduces the total supply.
82: /// @dev Emits a {Transfer} event with `to` set to the zero address.
83: /// @dev `_owner` cannot be the zero address.
84: /// @dev `_owner` must have at least `_amount` tokens.
85: /// @param _owner The owner of the tokens
86: /// @param _amount The amount of tokens to burn
92: /// @notice Transfers tokens from one account to another.
93: /// @dev Emits a {Transfer} event.
94: /// @dev `_from` cannot be the zero address.
95: /// @dev `_to` cannot be the zero address.
96: /// @dev `_from` must have a balance of at least `_amount`.
97: /// @param _from Address to transfer from
98: /// @param _to Address to transfer to
99: /// @param _amount The amount of tokens to transfer
```
[10](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L10) | [11](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L11) | [12](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L12) | [13](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L13) | [14](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L14) | [15](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L15) | [16](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L16) | [17](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L17) | [18](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L18) | [21](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L21) | [23](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L23) | [25](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L25) | [28](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L28) | [30](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L30) | [32](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L32) | [35](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L35) | [37](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L37) | [39](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L39) | [40](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L40) | [41](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L41) | [51](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L51) | [53](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L53) | [55](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L55) | [62](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L62) | [67](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L67) | [69](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L69) | [71](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L71) | [72](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L72) | [73](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L73) | [74](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L74) | [75](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L75) | [81](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L81) | [82](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L82) | [83](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L83) | [84](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L84) | [85](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L85) | [86](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L86) | [92](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L92) | [93](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L93) | [94](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L94) | [95](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L95) | [96](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L96) | [97](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L97) | [98](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L98) | [99](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L99)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

9: /// @title  PoolTogether V5 Claimable Vault Extension
10: /// @author G9 Software Inc.
11: /// @notice Provides an interface for Claimer contracts to interact with a vault in PoolTogether
12: /// V5 while allowing each account to set and manage prize hooks that are called when they win.
15: ////////////////////////////////////////////////////////////////////////////////
17: ////////////////////////////////////////////////////////////////////////////////
19: /// @notice The gas to give to each of the before and after prize claim hooks.
20: /// @dev This should be enough gas to mint an NFT if needed.
23: /// @notice Address of the PrizePool that computes prizes.
26: /// @notice Address of the claimer.
29: ////////////////////////////////////////////////////////////////////////////////
31: ////////////////////////////////////////////////////////////////////////////////
33: /// @notice Thrown when the Prize Pool is set to the zero address.
36: /// @notice Thrown when the Claimer is set to the zero address.
39: /// @notice Thrown when a prize is claimed for the zero address.
42: /// @notice Thrown when the caller is not the prize claimer.
43: /// @param caller The caller address
44: /// @param claimer The claimer address
47: ////////////////////////////////////////////////////////////////////////////////
49: ////////////////////////////////////////////////////////////////////////////////
51: /// @notice Requires the caller to be the claimer.
57: ////////////////////////////////////////////////////////////////////////////////
59: ////////////////////////////////////////////////////////////////////////////////
61: /// @notice Claimable constructor
62: /// @param prizePool_ The prize pool to claim prizes from
63: /// @param claimer_ The address allowed to claim prizes on behalf of winners
70: ////////////////////////////////////////////////////////////////////////////////
72: ////////////////////////////////////////////////////////////////////////////////
74: /// @inheritdoc IClaimable
75: /// @dev Also calls the before and after claim hooks if set by the winner.
121: ////////////////////////////////////////////////////////////////////////////////
123: ////////////////////////////////////////////////////////////////////////////////
125: /// @notice Set claimer address.
126: /// @dev Will revert if `_claimer` is address zero.
127: /// @param _claimer Address of the claimer
```
[9](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L9) | [10](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L10) | [11](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L11) | [12](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L12) | [15](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L15) | [17](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L17) | [19](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L19) | [20](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L20) | [23](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L23) | [26](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L26) | [29](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L29) | [31](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L31) | [33](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L33) | [36](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L36) | [39](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L39) | [42](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L42) | [43](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L43) | [44](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L44) | [47](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L47) | [49](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L49) | [51](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L51) | [57](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L57) | [59](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L59) | [61](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L61) | [62](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L62) | [63](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L63) | [70](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L70) | [72](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L72) | [74](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L74) | [75](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L75) | [121](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L121) | [123](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L123) | [125](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L125) | [126](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L126) | [127](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L127)
</details>

### [N-20] Unused `error` definition

Custom errors that aren't utilized within the contract add unnecessary complexity to the codebase. 
Cleaning up and removing these unused custom errors can enhance the readability and maintainability of the contract.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit Custom Error `SweepZeroAssets` declared but never used
206: error SweepZeroAssets();
```
[206](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L206)
</details>

### [N-21] Critical system parameter changes should be behind a timelock

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user).

<details>
<summary><i>4 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

735: function setClaimer(address _claimer) external onlyOwner {
742: function setLiquidationPair(address _liquidationPair) external onlyOwner {
753: function setYieldFeePercentage(uint32 _yieldFeePercentage) external onlyOwner {
759: function setYieldFeeRecipient(address _yieldFeeRecipient) external onlyOwner {
```
[735](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L735) | [742](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L742) | [753](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L753) | [759](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L759)
</details>

### [N-22] Outdated Solidity Version

The current Solidity version used in the contract is outdated.
Consider using a more recent version for improved features and security.

0.8.4: bytes.concat() instead of abi.encodePacked(,)

0.8.12: string.concat() instead of abi.encodePacked(,)

0.8.13: Ability to use using for with a list of free functions

0.8.14:
    ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against calldatasize() in all cases. Override Checker: Allow changing data location for parameters only when overriding external functions.

0.8.15:
    Code Generation: Avoid writing dirty bytes to storage when copying bytes arrays. Yul Optimizer: Keep all memory side-effects of inline assembly blocks.

0.8.16:
    Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.

0.8.17:
    Yul Optimizer: Prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

2: pragma solidity ^0.8.0;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L2)
</details>

### [N-23] Ensure Non-Empty Check for Bytes in Function Parameters

To avoid mistakenly accepting empty bytes as valid parameters, it is advisable to implement checks for non-empty bytes within functions.
This ensures that empty bytes are not treated as valid inputs, preventing potential issues.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

524: function depositWithPermit(
        uint256 _assets,
        address _owner,
        uint256 _deadline,
        uint8 _v,
        bytes32 _r,
        bytes32 _s
    ) external returns (uint256) {
524: function depositWithPermit(
        uint256 _assets,
        address _owner,
        uint256 _deadline,
        uint8 _v,
        bytes32 _r,
        bytes32 _s
    ) external returns (uint256) {
```
[524](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L524) | [524](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L524)
</details>

### [N-24] Function/Constructor Argument Names Not in mixedCase

Underscore before of after function argument names is a common convention in Solidity NOT a documentation requirement.

Function arguments should use mixedCase for better readability and consistency with Solidity style guidelines. 
Examples of good practice include: initialSupply, account, recipientAddress, senderAddress, newOwner. 
[More information in Documentation](https://docs.soliditylang.org/en/v0.8.20/style-guide.html#function-argument-names)

Rule exceptions
- Allow constant variable name/symbol/decimals to be lowercase (ERC20).
- Allow `_` at the beginning of the mixedCase match for `private variables` and `unused parameters`.

<details>
<summary><i>44 issue instances in 4 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

341: function convertToShares(uint256 _assets) public view returns (uint256) {
355: function convertToAssets(uint256 _shares) public view returns (uint256) {
397: function maxMint(address _owner) public view returns (uint256) {
404: function maxWithdraw(address _owner) public view returns (uint256) {
415: function maxRedeem(address _owner) public view returns (uint256) {
441: function previewDeposit(uint256 _assets) public pure returns (uint256) {
447: function previewMint(uint256 _shares) public pure returns (uint256) {
454: function previewWithdraw(uint256 _assets) public view returns (uint256) {
470: function previewRedeem(uint256 _shares) public view returns (uint256) {
475: function deposit(uint256 _assets, address _receiver) external returns (uint256) {
482: function mint(uint256 _shares, address _receiver) external returns (uint256) {
489: function withdraw(
        uint256 _assets,
        address _receiver,
        address _owner
    ) external returns (uint256) {
500: function redeem(
        uint256 _shares,
        address _receiver,
        address _owner
    ) external returns (uint256) {
524: function depositWithPermit(
        uint256 _assets,
        address _owner,
        uint256 _deadline,
        uint8 _v,
        bytes32 _r,
        bytes32 _s
    ) external returns (uint256) {
552: function sponsor(uint256 _assets) external returns (uint256) {
611: function claimYieldFeeShares(uint256 _shares) external onlyYieldFeeRecipient {
631: function liquidatableBalanceOf(address _tokenOut) public view returns (uint256) {
659: function transferTokensOut(
        address,
        address _receiver,
        address _tokenOut,
        uint256 _amountOut
    ) public virtual onlyLiquidationPair returns (bytes memory) {
703: function verifyTokensIn(
        address _tokenIn,
        uint256 _amountIn,
        bytes calldata
    ) external onlyLiquidationPair {
722: function isLiquidationPair(
        address _tokenOut,
        address _liquidationPair
    ) external view returns (bool) {
735: function setClaimer(address _claimer) external onlyOwner {
742: function setLiquidationPair(address _liquidationPair) external onlyOwner {
753: function setYieldFeePercentage(uint32 _yieldFeePercentage) external onlyOwner {
759: function setYieldFeeRecipient(address _yieldFeeRecipient) external onlyOwner {
772: function _tryGetAssetDecimals(IERC20 asset_) internal view returns (bool, uint8) {
790: function _totalDebt(uint256 _totalSupply) internal view returns (uint256) {
798: function _twabSupplyLimit(uint256 _totalSupply) internal pure returns (uint256) {
808: function _totalYieldBalance(uint256 _totalAssets, uint256 totalDebt_) internal pure returns (uint256) {
823: function _availableYieldBalance(uint256 _totalAssets, uint256 totalDebt_) internal view returns (uint256) {
843: function _depositAndMint(address _caller, address _receiver, uint256 _assets, uint256 _shares) internal {
887: function _burnAndWithdraw(
        address _caller,
        address _receiver,
        address _owner,
        uint256 _shares,
        uint256 _assets
    ) internal {
928: function _withdraw(address _receiver, uint256 _assets) internal {
947: function _setYieldFeePercentage(uint32 _yieldFeePercentage) internal {
958: function _setYieldFeeRecipient(address _yieldFeeRecipient) internal {
289: constructor(
        string memory name_,
        string memory symbol_,
        IERC4626 yieldVault_,
        PrizePool prizePool_,
        address claimer_,
        address yieldFeeRecipient_,
        uint32 yieldFeePercentage_,
        uint256 yieldBuffer_,
        address owner_
    ) TwabERC20(name_, symbol_, prizePool_.twabController()) Claimable(prizePool_, claimer_) Ownable(owner_) {
```
[341](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L341) | [355](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L355) | [397](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L397) | [404](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L404) | [415](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L415) | [441](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L441) | [447](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L447) | [454](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L454) | [470](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L470) | [475](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L475) | [482](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L482) | [489](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L489) | [500](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L500) | [524](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L524) | [552](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L552) | [611](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L611) | [631](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L631) | [659](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L659) | [703](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L703) | [722](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L722) | [735](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L735) | [742](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L742) | [753](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L753) | [759](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L759) | [772](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L772) | [790](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L790) | [798](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L798) | [808](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L808) | [823](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L823) | [843](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L843) | [887](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L887) | [928](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L928) | [947](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L947) | [958](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L958) | [289](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L289)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

92: function deployVault(
      string memory _name,
      string memory _symbol,
      IERC4626 _yieldVault,
      PrizePool _prizePool,
      address _claimer,
      address _yieldFeeRecipient,
      uint32 _yieldFeePercentage,
      address _owner
    ) external returns (PrizeVault) {
```
[92](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L92)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

56: function balanceOf(
        address _account
    ) public view virtual override(ERC20) returns (uint256) {
76: function _mint(address _receiver, uint256 _amount) internal virtual override {
87: function _burn(address _owner, uint256 _amount) internal virtual override {
100: function _transfer(address _from, address _to, uint256 _amount) internal virtual override {
42: constructor(
        string memory name_,
        string memory symbol_,
        TwabController twabController_
    ) ERC20(name_, symbol_) ERC20Permit(name_) {
```
[56](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L56) | [76](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L76) | [87](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L87) | [100](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L100) | [42](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L42)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

76: function claimPrize(
        address _winner,
        uint8 _tier,
        uint32 _prizeIndex,
        uint96 _reward,
        address _rewardRecipient
    ) external onlyClaimer returns (uint256) {
128: function _setClaimer(address _claimer) internal {
64: constructor(PrizePool prizePool_, address claimer_) {
```
[76](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L76) | [128](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L128) | [64](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L64)
</details>

### [N-25] Upgrade `openzeppelin` to the Latest Version - 5.0.0

These contracts import contracts from @openzeppelin/contracts but they are not using the latest version.
For more information, please visit: [OpenZeppelin GitHub Releases](https://github.com/OpenZeppelin/openzeppelin-contracts/releases)
It is recommended to always use the latest version to take advantage of updates and security fixes.

<details>
<summary><i>8 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

4: import { IERC4626 } from "openzeppelin/interfaces/IERC4626.sol";
5: import { SafeERC20, IERC20Permit } from "openzeppelin/token/ERC20/utils/SafeERC20.sol";
6: import { ERC20, IERC20, IERC20Metadata } from "openzeppelin/token/ERC20/ERC20.sol";
7: import { Math } from "openzeppelin/utils/math/Math.sol";
```
[4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L4) | [5](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L5) | [6](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L6) | [7](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L7)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

4: import { IERC20, IERC4626 } from "openzeppelin/token/ERC20/extensions/ERC4626.sol";
```
[4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L4)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

4: import { ERC20 } from "openzeppelin/token/ERC20/ERC20.sol";
5: import { ERC20Permit } from "openzeppelin/token/ERC20/extensions/ERC20Permit.sol";
6: import { SafeCast } from "openzeppelin/utils/math/SafeCast.sol";
```
[4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L4) | [5](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L5) | [6](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L6)
</details>

### [N-26] Duplicated `require()`/`revert()` checks should be refactored to a modifier or function

Duplicated require() or revert() checks should be refactored to a modifier or function.
This helps in maintaining a clean and organized codebase and saves deployment costs.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

612: if (_shares == 0) revert MintZeroShares();
844: if (_shares == 0) revert MintZeroShares();
```
[612](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L612) | [844](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L844)
</details>

### [N-27] Event is not properly `indexed`

Index event fields make the field more quickly accessible to off-chain tools that parse events.
This is especially useful when it comes to filtering based on an address. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields).
Where applicable, each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question.
If there are fewer than three applicable fields, all of the applicable fields should be indexed.

<details>
<summary><i>4 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

150: event YieldFeePercentageSet(uint256 yieldFeePercentage);
156: event Sponsor(address indexed caller, uint256 assets, uint256 shares);
175: event ClaimYieldFeeShares(address indexed recipient, uint256 shares);
```
[150](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L150) | [156](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L156) | [175](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L175)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

14: event SetHooks(address indexed account, VaultHooks hooks);
```
[14](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L14)
</details>

### [N-28] Consider Using `safePermit()` Instead of `permit()`

The use of the `permit()` function can be risky as observed in the Anyswap hack where the function didn't really exist, but the fallback function didn't revert the transaction. In particular, tokens like WETH, BNB, and HEX exhibit a 'phantom permit'—they accept any call to `permit()` without reverting, posing a security risk.

To mitigate this risk, consider using `safePermit()` which ensures that the `permit` function call actually goes through as intended.

For more details, see [Phantom Functions and the Billion Dollar No-op](https://media.dedaub.com/phantom-functions-and-the-billion-dollar-no-op-c56f062ae49f#ef54).

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

540: IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);
```
[540](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L540)
</details>

### [N-29] Consider Adding Emergency-Stop Functionality

Smart contracts that hold significant value, interact with external contracts, or have complex logic should include an emergency-stop mechanism for added security. This allows pausing certain contract functionalities in case of emergencies, mitigating potential damages.

This contract seems to lack such a mechanism. Implementing an emergency stop can enhance contract security and reliability.

<details>
<summary><i>2 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

1: No emergency stop pattern found
```
[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L1)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

1: No emergency stop pattern found
```
[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L1)
</details>

### [N-30] Consider adding a block/deny-list

While adding a block or deny-list may increase the level of centralization in the contract, it provides an additional layer of security by preventing hackers from using stolen tokens or carrying out other malicious activities.

Although it's a trade-off, a block or deny-list can help improve the overall security posture of the contract.

<details>
<summary><i>2 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

65: contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownable {
```
[65](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L65)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

19: contract TwabERC20 is ERC20, ERC20Permit {
```
[19](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L19)
</details>

### [N-31] Non-uppercase/Constant-case Naming for `immutable` Variables

For better readability and adherence to common naming conventions, variable names declared as immutable should be written in all uppercase letters.

<details>
<summary><i>6 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

112: uint256 public immutable yieldBuffer;
115: IERC4626 public immutable yieldVault;
135: IERC20 private immutable _asset;
138: uint8 private immutable _underlyingDecimals;
```
[112](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L112) | [115](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L115) | [135](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L135) | [138](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L138)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

26: TwabController public immutable twabController;
```
[26](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L26)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

24: PrizePool public immutable prizePool;
```
[24](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L24)
</details>

### [N-32] Insufficient Comments in Complex Functions

Complex functions spanning multiple lines should have more comments to better explain the purpose of each logic step.
Proper commenting can greatly improve the readability and maintainability of the codebase. Consider adding explicit comments
to provide insights into the function's operation.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

/// @audit function with 31 lines has only 0 comment lines
76: function claimPrize(
        address _winner,
        uint8 _tier,
        uint32 _prizeIndex,
        uint96 _reward,
        address _rewardRecipient
    ) external onlyClaimer returns (uint256) {
```
[76](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L76)
</details>

### [N-33] `constant`s should be defined rather than using magic numbers

Magic numbers are hardcoded numerical values used directly in the code, making it harder to read and maintain.
Consider using constants instead of magic numbers.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit 18
305: _underlyingDecimals = success ? assetDecimals : 18;
/// @audit 32
776: if (success && encodedDecimals.length >= 32) {
```
[305](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L305) | [776](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L776)
</details>

### [N-34] Consider moving `msg.sender` checks to a common authorization `modifier`

Functions that are only allowed to be called by a specific actor should use a modifier to check if the caller is the specified actor (e.g., owner, specific role, etc.).
Using `require` to check `msg.sender` in the function body is less efficient and less clear.

Consider refactoring these `require` statements into a modifier for better readability, organization, and gas efficiency.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

532: if (_owner != msg.sender) {
```
[532](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L532)
</details>

### [N-35] NatSpec: Contract declarations should have `@author` tag

In the world of decentralized code, giving credit is key. NatSpec's `@author` tag acknowledges the minds behind the code.
It appears this Solidity contract omits the `@author` directive in its NatSpec annotations.
Properly attributing code to its contributors not only recognizes effort but also aids in establishing trust and credibility.
[Dive Deeper into NatSpec Guidelines](https://docs.soliditylang.org/en/develop/natspec-format.html)

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

65: contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownable {
```
[65](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L65)
</details>

### [N-36] Consider defining system-wide constants in a single file

System-wide constants should be declared in a single file for better maintainability and readability.
This contract seems to contain constants which could potentially be system-wide and could be better managed if they were centralized in a single location.

<details>
<summary><i>4 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

74: uint32 public constant FEE_PRECISION = 1e9;
80: uint32 public constant MAX_YIELD_FEE = 9e8;
```
[74](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L74) | [80](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L80)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

63: uint256 public constant YIELD_BUFFER = 1e5;
```
[63](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L63)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

21: uint24 public constant HOOK_GAS = 150_000;
```
[21](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L21)
</details>

### [N-37] Add inline comments for unnamed variables

Having unnamed variables in function definitions can be confusing and decrease the code readability. 
Adding inline comments to explain the purpose of unnamed variables could make the code easier to understand and maintain.

For example, convert function declarations like `function foo(address x, address)` to `function foo(address x, address /* y */)` to indicate that the unnamed variable could have been named 'y'.

<details>
<summary><i>4 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit - `address`
374: function maxDeposit(address) public view returns (uint256) {
/// @audit - `address`
659: function transferTokensOut(
        address,
        address _receiver,
        address _tokenOut,
        uint256 _amountOut
    ) public virtual onlyLiquidationPair returns (bytes memory) {
/// @audit - `bytes`
703: function verifyTokensIn(
        address _tokenIn,
        uint256 _amountIn,
        bytes calldata
    ) external onlyLiquidationPair {
/// @audit - `address`
717: function targetOf(address) external view returns (address) {
```
[374](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L374) | [659](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L659) | [703](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L703) | [717](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L717)
</details>

### [N-38] Consider making contracts `Upgradeable`

Contract uses a non-upgradeable design.
Transitioning to an upgradeable contract structure is more aligned with contemporary smart contract practices.
This approach not only enhances flexibility but also allows for continuous improvement and adaptation, ensuring the contract stays relevant and robust in an ever-evolving ecosystem.

<details>
<summary><i>5 issue instances in 5 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit - Contract `PrizeVault` is not upgradeable
65: contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownable {
```
[65](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L65)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

/// @audit - Contract `PrizeVaultFactory` is not upgradeable
13: contract PrizeVaultFactory {
```
[13](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L13)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

/// @audit - Contract `TwabERC20` is not upgradeable
19: contract TwabERC20 is ERC20, ERC20Permit {
```
[19](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L19)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

/// @audit - Contract `Claimable` is not upgradeable
13: abstract contract Claimable is HookManager, IClaimable {
```
[13](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L13)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

/// @audit - Contract `HookManager` is not upgradeable
9: abstract contract HookManager {
```
[9](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L9)
</details>

### [N-39] Events that mark critical parameter changes should contain both the old and the new value

When emitting events that signify critical changes in parameters, it's important to include both the old and new values. 

This aids in auditability, providing more complete information about the state change. It's especially necessary when the new value isn't required to be different from the old one, as this prevents ambiguity in event logs.

<details>
<summary><i>2 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

952: emit YieldFeePercentageSet(_yieldFeePercentage);
```
[952](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L952)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

131: emit ClaimerSet(_claimer);
```
[131](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L131)
</details>

### [N-40] Style guide: Contract does not follow the Solidity style guide's suggested layout ordering

Adhering to a recommended order in Solidity contracts enhances code readability and maintenance.
[More information in Documentation](https://docs.soliditylang.org/en/latest/style-guide.html#order-of-layout)
It's recommended to use the following order:
1. Type declarations
2. State variables
3. Events
4. Errors
5. Modifiers
6. Functions

<details>
<summary><i>2 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

/// @audit `state variable` declared after `event`
63: uint256 public constant YIELD_BUFFER = 1e5;
```
[63](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L63)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

/// @audit `state variable` declared after `event`
17: mapping(address => VaultHooks) internal _hooks;
```
[17](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L17)
</details>

### [N-41] Style guide: Control structures do not follow the Solidity Style Guide

Following best practices for control structures in solidity code is vital for readability and maintainability. The control structures in contracts, libraries, functions, and structs should adhere to the following standards:

- Braces denoting the body should open on the same line as the declaration and close on their own line at the same indentation level as the beginning of the declaration. 
- A single space should precede the opening brace. 
- Control structures such as 'if', 'else', 'while', and 'for' should also follow these spacing and brace placement recommendations.

It is advised to revisit the [control structures](https://docs.soliditylang.org/en/latest/style-guide.html#control-structures) sections in documentation to ensure conformity with these best practices, fostering cleaner and more maintainable code.

<details>
<summary><i>18 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit `Return or revert statement should be on new line.`
300: if (address(yieldVault_) == address(0)) revert YieldVaultZeroAddress();
/// @audit `Return or revert statement should be on new line.`
301: if (owner_ == address(0)) revert OwnerZeroAddress();
/// @audit `Return or revert statement should be on new line.`
377: if (totalAssets() < totalDebt_) return 0;
/// @audit `Return or revert statement should be on new line.`
458: if (_totalAssets == 0) revert ZeroTotalAssets();
/// @audit `Return or revert statement should be on new line.`
612: if (_shares == 0) revert MintZeroShares();
/// @audit `Return or revert statement should be on new line.`
615: if (_shares > _yieldFeeBalance) revert SharesExceedsYieldFeeBalance(_shares, _yieldFeeBalance);
/// @audit `Return or revert statement should be on new line.`
665: if (_amountOut == 0) revert LiquidationAmountOutZero();
/// @audit `Return or revert statement should be on new line.`
743: if (address(_liquidationPair) == address(0)) revert LPZeroAddress();
/// @audit `Return or revert statement should be on new line.`
844: if (_shares == 0) revert MintZeroShares();
/// @audit `Return or revert statement should be on new line.`
845: if (_assets == 0) revert DepositZeroAssets();
/// @audit `Return or revert statement should be on new line.`
873: if (totalAssets() < totalDebt()) revert LossyDeposit(totalAssets(), totalDebt());
/// @audit `Return or revert statement should be on new line.`
894: if (_assets == 0) revert WithdrawZeroAssets();
/// @audit `Return or revert statement should be on new line.`
895: if (_shares == 0) revert BurnZeroShares();
```
[300](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L300) | [301](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L301) | [377](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L377) | [458](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L458) | [612](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L612) | [615](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L615) | [665](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L665) | [743](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L743) | [844](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L844) | [845](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L845) | [873](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L873) | [894](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L894) | [895](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L895)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

/// @audit `Return or revert statement should be on new line.`
47: if (address(0) == address(twabController_)) revert TwabControllerZeroAddress();
```
[47](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L47)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

/// @audit `Return or revert statement should be on new line.`
53: if (msg.sender != claimer) revert CallerNotClaimer(msg.sender, claimer);
/// @audit `Return or revert statement should be on new line.`
65: if (address(prizePool_) == address(0)) revert PrizePoolZeroAddress();
/// @audit `Return or revert statement should be on new line.`
95: }

        if (recipient == address(0)) revert ClaimRecipientZeroAddress();
/// @audit `Return or revert statement should be on new line.`
129: if (_claimer == address(0)) revert ClaimerZeroAddress();
```
[53](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L53) | [65](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L65) | [95](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L95) | [129](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L129)
</details>

### [N-42] Constants in comparisons should appear on the left side

Placing constants on the left side of comparison statements can help prevent accidental assignments and improve code readability.
In languages like C, placing constants on the left can protect against unintended assignments that would be treated as true conditions, leading to bugs.
Although Solidity does not have this specific issue, using the same practice can still be beneficial for code readability and consistency.

Consider placing constants on the left side of comparison operators like `==`, `!=`, `<`, `>`, `<=`, and `>=`."

<details>
<summary><i>10 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

458: if (_totalAssets == 0) revert ZeroTotalAssets();
612: if (_shares == 0) revert MintZeroShares();
665: if (_amountOut == 0) revert LiquidationAmountOutZero();
672: if (_yieldFeePercentage != 0) {
684: if (_yieldFee > 0) {
776: if (success && encodedDecimals.length >= 32) {
844: if (_shares == 0) revert MintZeroShares();
845: if (_assets == 0) revert DepositZeroAssets();
894: if (_assets == 0) revert WithdrawZeroAssets();
895: if (_shares == 0) revert BurnZeroShares();
```
[458](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L458) | [612](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L612) | [665](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L665) | [672](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L672) | [684](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L684) | [776](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L776) | [844](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L844) | [845](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L845) | [894](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L894) | [895](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L895)
</details>

### [N-43] NatSpec: Contract declarations should have `@notice` tag

The `@notice` tag in NatSpec annotations serves to provide information about the contract's functionality that is visible to end-users.
Including `@notice` comments helps to improve the contract's transparency and ease of understanding, contributing to the overall trust and credibility of the code.
[NatSpec Guidelines](https://docs.soliditylang.org/en/develop/natspec-format.html)

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

65: contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownable {
```
[65](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L65)
</details>

### [N-44] Long functions should be refactored into multiple, smaller, functions

Functions that span many lines can be hard to understand and maintain.
It is often beneficial to refactor long functions into multiple smaller functions.
This improves readability, makes testing easier, and can even lead to gas optimization if it eliminates the need for variables that would otherwise have to be stored in memory.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

 /// @audit 31 lines (excluding comments)
76: function claimPrize(
        address _winner,
        uint8 _tier,
        uint32 _prizeIndex,
        uint96 _reward,
        address _rewardRecipient
    ) external onlyClaimer returns (uint256) {
```
[76](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L76)
</details>

### [N-45] NatSpec: Contract declarations should have `@dev` tags

NatSpec comments are a critical part of Solidity's documentation system, designed to help developers and others understand the behavior and purpose of a contract.
The `@dev` tag, in particular, provides context and insight into the contract's development considerations.
A missing `@dev` comment can lead to misunderstandings about the contract, making it harder for others to contribute to or use the contract effectively.
Therefore, it's highly recommended to include `@dev` comments in the documentation to enhance code readability and maintainability.
[Refer to NatSpec Documentation](https://docs.soliditylang.org/en/develop/natspec-format.html)

<details>
<summary><i>2 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

13: contract PrizeVaultFactory {
```
[13](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L13)

```solidity
File: pt-v5-vault/src/interfaces/IVaultHooks.sol

17: interface IVaultHooks {
```
[17](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol#L17)
</details>

### [N-46] `public` functions not called by the contract should be declared `external` instead

Contracts are allowed to override their parents' functions and change the visibility from `external` to `public`.
If a `public` function is not called internally within the contract, it should be declared as `external` to save gas.

<details>
<summary><i>6 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

320: function decimals() public view override(ERC20, IERC20Metadata) returns (uint8) {
341: function convertToShares(uint256 _assets) public view returns (uint256) {
397: function maxMint(address _owner) public view returns (uint256) {
404: function maxWithdraw(address _owner) public view returns (uint256) {
584: function totalYieldBalance() public view returns (uint256) {
631: function liquidatableBalanceOf(address _tokenOut) public view returns (uint256) {
```
[320](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L320) | [341](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L341) | [397](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L397) | [404](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L404) | [584](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L584) | [631](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L631)
</details>

### [N-47] `else`-block not required

In Solidity, if an `if` block contains a `return` statement, subsequent `else` blocks become unnecessary.
This is because the `return` statement halts the execution of the function at that point, making any `else` conditions redundant.
Therefore, omitting `else` after such `if` blocks can lead to cleaner and more readable code without any change in the functionality.

<details>
<summary><i>7 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

344: if (_totalAssets >= totalDebt_) {
            return _assets;
        } else {
            // If the vault controls less assets than what has been deposited a share will be worth a
            // proportional amount of the total assets. This can happen due to fees, slippage, or loss
            // of funds in the underlying yield vault.
            return _assets.mulDiv(totalDebt_, _totalAssets, Math.Rounding.Down);
        }
358: if (_totalAssets >= totalDebt_) {
            return _shares;
        } else {
            // If the vault controls less assets than what has been deposited a share will be worth a
            // proportional amount of the total assets. This can happen due to fees, slippage, or loss
            // of funds in the underlying yield vault.
            return _shares.mulDiv(_totalAssets, totalDebt_, Math.Rounding.Down);
        }
384: if (_latentBalance >= _maxYieldVaultDeposit) {
            return 0;
        } else {
            unchecked {
                _maxDeposit = _maxYieldVaultDeposit - _latentBalance;
            }
422: if (_ownerShares > _maxWithdraw) {
            uint256 _totalAssets = totalAssets();
            uint256 totalDebt_ = totalDebt();
            if (_totalAssets >= totalDebt_) {
                return _maxWithdraw;
            } else {
                // Convert to shares while rounding up. Since 1 asset is guaranteed to be worth more than
                // 1 share and any upwards rounding will not exceed 1 share, we can be sure that when the
                // shares are converted back to assets (rounding down) the resulting asset value won't
                // exceed `_maxWithdraw`.
                uint256 _maxScaledRedeem = _maxWithdraw.mulDiv(totalDebt_, _totalAssets, Math.Rounding.Up);
                return _maxScaledRedeem >= _ownerShares ? _ownerShares : _maxScaledRedeem;
            }
461: if (_totalAssets >= totalDebt_) {
            return _assets;
        } else {
            // Follows the inverse conversion of `convertToAssets`
            return _assets.mulDiv(totalDebt_, _totalAssets, Math.Rounding.Up);
        }
600: if (totalYieldBalance_ >= _yieldBuffer) {
            return _yieldBuffer;
        } else {
            return totalYieldBalance_;
        }
809: if (totalDebt_ >= _totalAssets) {
            return 0;
        } else {
            unchecked {
                return _totalAssets - totalDebt_;
            }
```
[344](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L344) | [358](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L358) | [384](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L384) | [422](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L422) | [461](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L461) | [600](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L600) | [809](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L809)
</details>

### [N-48] Style guide: Function ordering does not follow the Solidity style guide

Ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier.
But there are contracts in the project that do not comply with this.
Functions should be grouped according to their visibility and ordered:
- constructor 
- receive function (if exists)
- fallback function (if exists)
- external
- public
- internal
- private.

<details>
<summary><i>8 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit `public` function `decimals()` declared before `external` function `asset()`
320: function decimals() public view override(ERC20, IERC20Metadata) returns (uint8) {
329: function asset() external view returns (address) {
/// @audit `public` function `previewRedeem()` declared before `external` function `deposit()`
470: function previewRedeem(uint256 _shares) public view returns (uint256) {
475: function deposit(uint256 _assets, address _receiver) external returns (uint256) {
/// @audit `public` function `availableYieldBalance()` declared before `external` function `currentYieldBuffer()`
591: function availableYieldBalance() public view returns (uint256) {
597: function currentYieldBuffer() external view returns (uint256) {
/// @audit `public` function `transferTokensOut()` declared before `external` function `verifyTokensIn()`
659: function transferTokensOut(
        address,
        address _receiver,
        address _tokenOut,
        uint256 _amountOut
    ) public virtual onlyLiquidationPair returns (bytes memory) {
703: function verifyTokensIn(
        address _tokenIn,
        uint256 _amountIn,
        bytes calldata
    ) external onlyLiquidationPair {
```
[320](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L320) | [329](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L329) | [470](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L470) | [475](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L475) | [591](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L591) | [597](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L597) | [659](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L659) | [703](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L703)
</details>

### [N-49] Style guide: Extraneous whitespace

See the [whitespace](https://docs.soliditylang.org/en/v0.8.24/style-guide.html#whitespace-in-expressions) section of the Solidity Style Guide

<details>
<summary><i>47 issue instances in 5 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
4: import { IERC4626 } from "openzeppelin/interfaces/IERC4626.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
5: import { SafeERC20, IERC20Permit } from "openzeppelin/token/ERC20/utils/SafeERC20.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
6: import { ERC20, IERC20, IERC20Metadata } from "openzeppelin/token/ERC20/ERC20.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
7: import { Math } from "openzeppelin/utils/math/Math.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
8: import { Ownable } from "owner-manager-contracts/Ownable.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
10: import { Claimable } from "./abstract/Claimable.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
11: import { TwabERC20 } from "./TwabERC20.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
13: import { ILiquidationSource } from "pt-v5-liquidator-interfaces/ILiquidationSource.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
14: import { PrizePool } from "pt-v5-prize-pool/PrizePool.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
15: import { TwabController, SPONSORSHIP_ADDRESS } from "pt-v5-twab-controller/TwabController.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
4: import { IERC4626 } from "openzeppelin/interfaces/IERC4626.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
5: import { SafeERC20, IERC20Permit } from "openzeppelin/token/ERC20/utils/SafeERC20.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
6: import { ERC20, IERC20, IERC20Metadata } from "openzeppelin/token/ERC20/ERC20.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
7: import { Math } from "openzeppelin/utils/math/Math.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
8: import { Ownable } from "owner-manager-contracts/Ownable.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
10: import { Claimable } from "./abstract/Claimable.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
11: import { TwabERC20 } from "./TwabERC20.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
13: import { ILiquidationSource } from "pt-v5-liquidator-interfaces/ILiquidationSource.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
14: import { PrizePool } from "pt-v5-prize-pool/PrizePool.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
15: import { TwabController, SPONSORSHIP_ADDRESS } from "pt-v5-twab-controller/TwabController.sol";
/// @audit `More than one space around an assignment or other operator to align with another`
647: uint256 _liquidYield = 
            _availableYieldBalance(totalAssets(), _totalDebt(_totalSupply))
```
[4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L4) | [5](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L5) | [6](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L6) | [7](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L7) | [8](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L8) | [10](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L10) | [11](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L11) | [13](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L13) | [14](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L14) | [15](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L15) | [4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L4) | [5](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L5) | [6](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L6) | [7](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L7) | [8](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L8) | [10](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L10) | [11](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L11) | [13](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L13) | [14](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L14) | [15](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L15) | [647](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L647)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
4: import { IERC20, IERC4626 } from "openzeppelin/token/ERC20/extensions/ERC4626.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
6: import { PrizePool } from "pt-v5-prize-pool/PrizePool.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
8: import { PrizeVault } from "./PrizeVault.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
4: import { IERC20, IERC4626 } from "openzeppelin/token/ERC20/extensions/ERC4626.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
6: import { PrizePool } from "pt-v5-prize-pool/PrizePool.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
8: import { PrizeVault } from "./PrizeVault.sol";
```
[4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L4) | [6](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L6) | [8](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L8) | [4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L4) | [6](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L6) | [8](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L8)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
4: import { ERC20 } from "openzeppelin/token/ERC20/ERC20.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
5: import { ERC20Permit } from "openzeppelin/token/ERC20/extensions/ERC20Permit.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
6: import { SafeCast } from "openzeppelin/utils/math/SafeCast.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
8: import { TwabController } from "pt-v5-twab-controller/TwabController.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
4: import { ERC20 } from "openzeppelin/token/ERC20/ERC20.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
5: import { ERC20Permit } from "openzeppelin/token/ERC20/extensions/ERC20Permit.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
6: import { SafeCast } from "openzeppelin/utils/math/SafeCast.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
8: import { TwabController } from "pt-v5-twab-controller/TwabController.sol";
```
[4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L4) | [5](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L5) | [6](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L6) | [8](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L8) | [4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L4) | [5](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L5) | [6](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L6) | [8](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L8)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
4: import { IClaimable } from "pt-v5-claimable-interface/interfaces/IClaimable.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
5: import { PrizePool } from "pt-v5-prize-pool/PrizePool.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
7: import { HookManager } from "./HookManager.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
86: recipient = _hooks[_winner].implementation.beforeClaimPrize{ gas: HOOK_GAS }(
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
109: _hooks[_winner].implementation.afterClaimPrize{ gas: HOOK_GAS }(
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
4: import { IClaimable } from "pt-v5-claimable-interface/interfaces/IClaimable.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
5: import { PrizePool } from "pt-v5-prize-pool/PrizePool.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
7: import { HookManager } from "./HookManager.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
86: recipient = _hooks[_winner].implementation.beforeClaimPrize{ gas: HOOK_GAS }(
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
109: _hooks[_winner].implementation.afterClaimPrize{ gas: HOOK_GAS }(
```
[4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L4) | [5](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L5) | [7](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L7) | [86](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L86) | [109](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L109) | [4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L4) | [5](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L5) | [7](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L7) | [86](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L86) | [109](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L109)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
4: import { VaultHooks } from "../interfaces/IVaultHooks.sol";
/// @audit `Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.`
4: import { VaultHooks } from "../interfaces/IVaultHooks.sol";
```
[4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L4) | [4](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L4)
</details>

### [N-50] Unused import

The contract contains import statements for libraries or other contracts that are not utilized within the code.
Excessive or unused imports can clutter the codebase, leading to inefficiency and potential confusion.
Consider removing any imports that are not essential to the contract's functionality.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit imported `TwabController` not used)
15: import { TwabController, SPONSORSHIP_ADDRESS } from "pt-v5-twab-controller/TwabController.sol";
```
[15](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L15)
</details>

### [N-51] `if`-statement can be converted to a ternary

The ternary operator provides a shorthand way to write if / else statements.
It can improve readability and reduce the number of lines of code.
Traditional `if / else` statements, particularly those spanning only a one lines, could potentially be refactored using the ternary operator.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

84: if (_hooks[_winner].useBeforeClaimPrize) {
            recipient = _hooks[_winner].implementation.beforeClaimPrize{ gas: HOOK_GAS }(
                _winner,
                _tier,
                _prizeIndex,
                _reward,
                _rewardRecipient
            );
        } else {
            recipient = _winner;
        }
```
[84](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L84)
</details>

### [N-52] Consider using named mappings

As of Solidity version 0.8.18, it is possible to use named mappings to clarify the purpose of each mapping in the code. 
It is recommended to use this feature for better code readability and maintainability.

More information: [Solidity 0.8.18 Release Announcement](https://blog.soliditylang.org/2023/02/01/solidity-0.8.18-release-announcement/)

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

17: mapping(address => VaultHooks) internal _hooks;
```
[17](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L17)
</details>

### [N-53] Use of `override` is unnecessary

In Solidity version 0.8.8 and later, the use of the `override` keyword becomes superfluous when a function is overriding solely from an interface and the function isn't present in multiple base contracts.
Previously, the `override` keyword was required as an explicit indication to the compiler. However, this is no longer the case, and the extraneous use of the keyword can make the code less clean and more verbose.

Solidity documentation on [Function Overriding](https://docs.soliditylang.org/en/v0.8.20/contracts.html#function-overriding).

<details>
<summary><i>6 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

320: function decimals() public view override(ERC20, IERC20Metadata) returns (uint8) {
```
[320](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L320)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

56: function balanceOf(
        address _account
    ) public view virtual override(ERC20) returns (uint256) {
63: function totalSupply() public view virtual override(ERC20) returns (uint256) {
76: function _mint(address _receiver, uint256 _amount) internal virtual override {
87: function _burn(address _owner, uint256 _amount) internal virtual override {
100: function _transfer(address _from, address _to, uint256 _amount) internal virtual override {
```
[56](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L56) | [63](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L63) | [76](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L76) | [87](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L87) | [100](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L100)
</details>

### [N-54] Setters should prevent re-setting of the same value

Setter functions should include a condition to check if the new value being assigned is different from the current value. This practice prevents redundant state changes and the emission of unnecessary events, ensuring that changes only occur when actual updates to the values are made.

This not only helps to maintain the integrity of the contract's state but also keeps the event logs cleaner and more meaningful by avoiding the recording of identical consecutive values. Such an approach can improve the clarity and efficiency of debugging and data tracking processes.

<details>
<summary><i>7 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit Missing `_claimer` check before state change
736: _setClaimer(_claimer);
/// @audit Missing `_liquidationPair` check before state change
744: liquidationPair = _liquidationPair;
/// @audit Missing `_yieldFeePercentage` check before state change
754: _setYieldFeePercentage(_yieldFeePercentage);
/// @audit Missing `_yieldFeeRecipient` check before state change
760: _setYieldFeeRecipient(_yieldFeeRecipient);
/// @audit Missing `_yieldFeePercentage` check before state change
951: yieldFeePercentage = _yieldFeePercentage;
/// @audit Missing `_yieldFeeRecipient` check before state change
959: yieldFeeRecipient = _yieldFeeRecipient;
```
[736](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L736) | [744](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L744) | [754](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L754) | [760](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L760) | [951](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L951) | [959](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L959)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

/// @audit Missing `_claimer` check before state change
130: claimer = _claimer;
```
[130](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L130)
</details>

### [N-55] NatSpec: Contract declarations should have `@title` tags

NatSpec comments offer an intuitive way to understand the core purpose of a smart contract. 
Especially, the `@title` tag serves as a brief descriptor of the contract's function or intent.
In this Solidity file, the `@title` directive seems to be overlooked.
Incorporating the `@title` tag gives readers a concise overview, thus enhancing clarity.
[Refer to NatSpec Documentation](https://docs.soliditylang.org/en/develop/natspec-format.html)

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

65: contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownable {
```
[65](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L65)
</details>

### [N-56] Contracts should have full test coverage

It's recommended to have full test coverage for all contracts.
While 100% code coverage does not guarantee absence of bugs, it can catch simple bugs and reduce regressions during code modifications.
Moreover, to achieve full coverage, authors often have to refactor their code into more modular components, each testable separately.
This leads to lower interdependencies, and results in code that is easier to understand and audit.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: app/2024-03-pooltogether

1: All files
```
[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/app/2024-03-pooltogether#L1)
</details>

### [N-57] Large or complicated code bases should implement invariant tests

Large or complex code bases should include invariant fuzzing tests, such as those provided by Echidna.
These tests require the identification of invariants that should not be violated under any circumstances, with the fuzzer testing various inputs and function calls to ensure these invariants always hold.
This is especially important for code with a lot of inline-assembly, complicated math, or complex interactions between contracts. Despite having 100% code coverage, bugs can still occur due to the order of operations a user performs.
Extensive and well-written invariant tests can significantly reduce this testing gap.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: app/2024-03-pooltogether

1: All files
```
[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/app/2024-03-pooltogether#L1)
</details>

### [N-58] Implement Formal Verification Proofs to Improve Security

Formal verification offers a mathematical proof confirming that your code operates as intended and is devoid of edge cases 
that may lead to unintended behavior. By leveraging this rigorous audit technique, you not only enhance the robustness 
of your code but also strengthen the trust of stakeholders in the safety of your contract.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: app/2024-03-pooltogether

1: All files
```
[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/app/2024-03-pooltogether#L1)
</details>

