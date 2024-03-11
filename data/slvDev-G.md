## Gas Findings

|    | Issue | Instances | Total Gas Saved |
|----|-------|:---------:|:---------:|
| [G-01] | Use `assembly` for Efficient Event Emission | 14 | 5320 |
| [G-02] | Enable IR-based code generation | 6 | - |
| [G-03] | Use `immutable`s variables directly, instead of cache them in stack | 2 | 0 |
| [G-04] | Assembly: Check `msg.sender` using xor and the scratch space | 4 | 84 |
| [G-05] | Multiple `address`/ID mappings can be combined into a single `mapping` of an `address`/ID to a `struct` | 2 | - |
| [G-06] | Assembly: Use scratch space when building emitted events with two data arguments | 3 | 45 |
| [G-07] | Stack variable is only used once | 4 | 12 |
| [G-08] | `abi.encodePacked `is more gas efficient than `abi.encode` | 1 | 200 |
| [G-09] | Use `assembly` to write mutable storage values | 6 | 66 |
| [G-10] | Use `revert()` to gain maximum gas savings | 24 | 1200 |
| [G-11] | `>=` costs less gas than `>` | 1 | 6 |
| [G-12] | Consider pre-calculating the address of `address(this)` to save gas | 25 | 0 |
| [G-13] | Call `msg.sender` directly instead of caching it | 1 | 14 |
| [G-14] | Use `unchecked` for Non-Loop Increment/Decrement Operations | 1 | 30 |
| [G-15] | Constructors can be marked `payable` | 3 | 72 |
| [G-16] | Reduce gas usage by moving to Solidity 0.8.19 or later | 1 | - |
| [G-17] | Reduce deployment costs by tweaking contracts' metadata | 6 | 63600 |
| [G-18] | Use Pre-Increment/Decrement (++i/--i) to Save Gas | 1 | 5 |
| [G-19] | Nesting `if`-statements is cheaper than using `&&` | 1 | 6 |
| [G-20] | Avoid transferring amounts of zero in order to save gas | 1 | 200 |
| [G-21] | `>=` costs less gas than `>` | 6 | 18 |
| [G-22] | Use `unchecked` for Math Operations if they already checked | 5 | 425 |
| [G-23] | Using `private` rather than `public`, saves gas | 16 | 352000 |
| [G-24] | Use Assembly for Hash Calculations | 1 | 1005 |
| [G-25] | Refactor duplicated require()/revert() checks to save gas | 2 | - |
| [G-26] | Use Assembly for Efficient Memory Management in Multiple External Calls | 10 | 26000 |
| [G-27] | Cached Global Variables | 1 | 12 |
| [G-28] | Simple checks for zero `uint` can be done using `assembly` to save gas | 8 | 32 |
| [G-29] | Consider using solady's `FixedPointMathLib` | 1 | - |
| [G-30] | Avoid Zero to Non-Zero Storage Writes Where Possible | 2 | 44200 |
| [G-31] | Use `unchecked` for division which do not divide by -X sonce they can't overflow | 1 | 20 |
| [G-32] | Use solady library where possible to save gas | 6 | 8000 |
| [G-33] | Use `uint256(1)`/`uint256(2)` instead of `true`/`false` to save gas for changes | 1 | 17000 |
| [G-34] | Mark Functions That Revert For Normal Users As `payable` | 8 | 168 |
| [G-35] | Prefer `private` over `public` for Constants to Save Gas | 4 | 12000 |
| [G-36] | `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables | 2 | 226 |
| [G-37] | Using `bool`s for storage incurs overhead | 1 | 100 |
| [G-38] | Optimize Gas by Using Only Named Returns | 40 | 1760 |
| [G-39] | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 19 | 114 |
| [G-40] | Use assembly to check for `address(0)` | 7 | 406 |
| [G-41] | `internal`/`private` functions only called once can be inlined to save gas | 2 | 40 |
| [G-42] | Declare `immutable` as `private` to save gas | 4 | 12000 |
| [G-43] | Optimize names to save gas | 5 | 100 |
| [G-44] | Avoid updating storage when the value hasn't changed | 7 | 5600 |
| [G-45] | Optimize External Calls with Assembly for Memory Efficiency | 24 | 5280 |

Total: 290 instances of 45 issues with 557366 gas saved

## Gas Findings Details

### [G-01] Use `assembly` for Efficient Event Emission

To efficiently emit events, consider utilizing assembly by making use of scratch space and the free memory pointer.
This approach can potentially avoid the costs associated with memory expansion.

However, it's crucial to cache and restore the free memory pointer for safe optimization.
Good examples of such practices can be found in well-optimized [Solady's codebases](https://github.com/Vectorized/solady/blob/main/src/tokens/ERC1155.sol#L167).
Please review your code and consider the potential gas savings of this approach.

<details>
<summary><i>14 issue instances in 5 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

562: emit Sponsor(_owner, _assets, _shares)
621: emit ClaimYieldFeeShares(msg.sender, _shares)
697: emit TransferYieldOut(msg.sender, _tokenOut, _receiver, _amountOut, _yieldFee)
747: emit LiquidationPairSet(address(this), address(_liquidationPair))
876: emit Deposit(_caller, _receiver, _assets, _shares)
909: emit Withdraw(_caller, _receiver, _owner, _assets, _shares)
952: emit YieldFeePercentageSet(_yieldFeePercentage)
960: emit YieldFeeRecipientSet(_yieldFeeRecipient)
```
[562](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L562) | [621](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L621) | [697](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L697) | [747](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L747) | [876](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L876) | [909](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L909) | [952](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L952) | [960](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L960)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

123: emit NewPrizeVault(
            _vault,
            _yieldVault,
            _prizePool,
            _name,
            _symbol
        )
```
[123](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L123)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

78: emit Transfer(address(0), _receiver, _amount)
89: emit Transfer(_owner, address(0), _amount)
102: emit Transfer(_from, _to, _amount)
```
[78](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L78) | [89](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L89) | [102](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L102)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

131: emit ClaimerSet(_claimer)
```
[131](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L131)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

31: emit SetHooks(msg.sender, hooks)
```
[31](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L31)
</details>

### [G-02] Enable IR-based code generation

The `--via-ir` command line option activates the IR-based code generator in Solidity, which is designed to enable powerful optimization passes that can span across functions. The end result may be a contract that requires less gas to execute its functions.

We recommend you enable this feature, run tests, and benchmark the gas usage of your contract to evaluate if it leads to any tangible gas savings. Experimenting with this feature could lead to a more gas-efficient contract.

[Solidity Documentation](https://docs.soliditylang.org/en/v0.8.20/ir-breaking-changes.html#solidity-ir-based-codegen-changes).

<details>
<summary><i>6 issue instances in 1 files:</i></summary>

```solidity

File: pt-v5-vault/src/PrizeVault.sol
File: pt-v5-vault/src/PrizeVaultFactory.sol
File: pt-v5-vault/src/TwabERC20.sol
File: pt-v5-vault/src/abstract/Claimable.sol
File: pt-v5-vault/src/abstract/HookManager.sol
File: pt-v5-vault/src/interfaces/IVaultHooks.sol
```
[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L1) | [1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L1) | [1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L1) | [1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L1) | [1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L1) | [1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol#L1)
</details>

### [G-03] Use `immutable`s variables directly, instead of cache them in stack

Caching `immutable`s variable in stack is not necessary, and it will cost more gas.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

599: uint256 _yieldBuffer = yieldBuffer
825: uint256 _yieldBuffer = yieldBuffer
```
[599](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L599) | [825](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L825)
</details>

### [G-04] Assembly: Check `msg.sender` using xor and the scratch space

See [this](https://code4rena.com/reports/2023-05-juicebox#g-06-use-assembly-to-validate-msgsender) prior finding for details on the conversion

<details>
<summary><i>4 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

261: if (msg.sender != liquidationPair) {
269: if (msg.sender != yieldFeeRecipient) {
532: if (_owner != msg.sender) {
```
[261](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L261) | [269](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L269) | [532](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L532)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

53: if (msg.sender != claimer) revert CallerNotClaimer(msg.sender, claimer);
```
[53](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L53)
</details>

### [G-05] Multiple `address`/ID mappings can be combined into a single `mapping` of an `address`/ID to a `struct`

Combining multiple address/ID mappings into a single mapping to a struct can lead to gas savings.
By refactoring multiple mappings into a singular mapping with a struct, you can save on storage slots, which in turn can reduce the gas cost in certain operations.
Prioritize this refactor if optimizing gas is a primary concern for your contract's operations.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

69: mapping(address vault => bool deployedByFactory) public deployedVaults
72: mapping(address deployer => uint256 nonce) public deployerNonces
```
[69](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L69) | [72](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L72)
</details>

### [G-06] Assembly: Use scratch space when building emitted events with two data arguments

Using the scratch space for more than one, but at most two words worth of data (non-indexed arguments) will save gas over needing Solidity's abi memory expansion used for emitting normally.

<details>
<summary><i>3 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

562: emit Sponsor(_owner, _assets, _shares)
697: emit TransferYieldOut(msg.sender, _tokenOut, _receiver, _amountOut, _yieldFee)
```
[562](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L562) | [697](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L697)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

123: emit NewPrizeVault(
            _vault,
            _yieldVault,
            _prizePool,
            _name,
            _symbol
        )
```
[123](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L123)
</details>

### [G-07] Stack variable is only used once

If the variable is only accessed once, it's cheaper to use the assigned value directly that one time, and save the 3 gas the extra stack assignment would spend

<details>
<summary><i>4 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit - `totalDebt_` variable
376: uint256 totalDebt_ = _totalDebt(_totalSupply)
/// @audit - `_yieldVaultShares` variable
865: uint256 _yieldVaultShares = yieldVault.previewDeposit(_assetsWithDust)
/// @audit - `_assetsUsed` variable
866: uint256 _assetsUsed = yieldVault.mint(_yieldVaultShares, address(this))
/// @audit - `_yieldVaultShares` variable
934: uint256 _yieldVaultShares = yieldVault.previewWithdraw(_assets - _latentAssets)
```
[376](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L376) | [865](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L865) | [866](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L866) | [934](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L934)
</details>

### [G-08] `abi.encodePacked `is more gas efficient than `abi.encode`

`abi.encode()` pads all elementary types to 32 bytes, whereas `abi.encodePacked()` will only use the minimal required memory to encode the data.
```solidity
    function testPacking() public pure returns (bytes memory) {
        // return abi.encode("string"); // 1120 gas
        // return abi.encodePacked("string"); // 913 gas
    }
```

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

103: abi.encode(msg.sender, deployerNonces[msg.sender]++)
```
[103](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L103)
</details>

### [G-09] Use `assembly` to write mutable storage values

Writing to storage using `assembly` is more gas efficient.
```solidity
    function writeStorage() external {
        // storageNumber = 10; // 2358 gas
        // assembly {
        //     sstore(storageNumber.slot, 10) // 2350 gas
        // }
        // storageAddr = 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc3; // 2411 gas
        // assembly {
        //     sstore(storageAddr.slot, 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc3) // 2350 gas
        // }
    }
```

<details>
<summary><i>6 issue instances in 4 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

745: liquidationPair = _liquidationPair
951: yieldFeePercentage = _yieldFeePercentage
959: yieldFeeRecipient = _yieldFeeRecipient
```
[745](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L745) | [951](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L951) | [959](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L959)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

121: deployedVaults[address(_vault)] = true
```
[121](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L121)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

130: claimer = _claimer
```
[130](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L130)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

30: _hooks[msg.sender] = hooks
```
[30](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L30)
</details>

### [G-10] Use `revert()` to gain maximum gas savings

If you dont need Error messages, or you want gain maximum gas savings - `revert()` is a cheapest way to revert transaction in terms of gas.
```solidity
    revert(); // 117 gas 
    require(false); // 132 gas
    revert CustomError(); // 157 gas
    assert(false); // 164 gas
    revert("Custom Error"); // 406 gas
    require(false, "Custom Error"); // 421 gas
```


<details>
<summary><i>24 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

262: revert CallerNotLP(msg.sender, liquidationPair)
270: revert CallerNotYieldFeeRecipient(msg.sender, yieldFeeRecipient)
300: revert YieldVaultZeroAddress()
301: revert OwnerZeroAddress()
458: revert ZeroTotalAssets()
533: revert PermitCallerNotOwner(msg.sender, _owner)
612: revert MintZeroShares()
615: revert SharesExceedsYieldFeeBalance(_shares, _yieldFeeBalance)
665: revert LiquidationAmountOutZero()
680: revert LiquidationExceedsAvailable(_amountOut + _yieldFee, _availableYield)
694: revert LiquidationTokenOutNotSupported(_tokenOut)
710: revert LiquidationTokenInNotPrizeToken(_tokenIn, _prizeToken)
743: revert LPZeroAddress()
844: revert MintZeroShares()
845: revert DepositZeroAssets()
874: revert LossyDeposit(totalAssets(), totalDebt())
894: revert WithdrawZeroAssets()
895: revert BurnZeroShares()
949: revert YieldFeePercentageExceedsMax(_yieldFeePercentage, MAX_YIELD_FEE)
```
[262](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L262) | [270](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L270) | [300](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L300) | [301](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L301) | [458](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L458) | [533](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L533) | [612](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L612) | [615](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L615) | [665](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L665) | [680](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L680) | [694](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L694) | [710](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L710) | [743](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L743) | [844](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L844) | [845](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L845) | [874](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L874) | [894](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L894) | [895](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L895) | [949](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L949)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

47: revert TwabControllerZeroAddress()
```
[47](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L47)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

53: revert CallerNotClaimer(msg.sender, claimer)
65: revert PrizePoolZeroAddress()
97: revert ClaimRecipientZeroAddress()
129: revert ClaimerZeroAddress()
```
[53](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L53) | [65](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L65) | [97](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L97) | [129](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L129)
</details>

### [G-11] `>=` costs less gas than `>`

The compiler uses opcodes GT and ISZERO for solidity code that uses >, but only requires LT for >=, which saves 3 gas. If < is being used, the condition can be inverted.
In cases where a for-loop is being used, one can count down rather than up, in order to use the optimal operator.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

684: if (_yieldFee > 0) {
```
[684](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L684)
</details>

### [G-12] Consider pre-calculating the address of `address(this)` to save gas

Use foundry's script.sol or solady's LibRlp.sol to save the value in a constant, which will avoid having to spend gas to push the value on the stack every time it's used.

<details>
<summary><i>25 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

337: return yieldVault.convertToAssets(yieldVault.balanceOf(address(this))) + _asset.balanceOf(address(this));
337: return yieldVault.convertToAssets(yieldVault.balanceOf(address(this))) + _asset.balanceOf(address(this));
382: uint256 _latentBalance = _asset.balanceOf(address(this));
383: uint256 _maxYieldVaultDeposit = yieldVault.maxDeposit(address(this));
405: uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));
416: uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));
539: if (_asset.allowance(_owner, address(this)) != _assets) {
540: IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);
558: if (twabController.delegateOf(address(this), _owner) != SPONSORSHIP_ADDRESS) {
634: if (_tokenOut == address(this)) {
639: _maxAmountOut = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));
691: } else if (_tokenOut == address(this)) {
713: prizePool.contributePrizeTokens(address(this), _amountIn);
726: return (_tokenOut == address(_asset) || _tokenOut == address(this)) && _liquidationPair == liquidationPair;
747: emit LiquidationPairSet(address(this), address(_liquidationPair));
856: address(this),
861: uint256 _assetsWithDust = _asset.balanceOf(address(this));
866: uint256 _assetsUsed = yieldVault.mint(_yieldVaultShares, address(this));
922: return yieldVault.convertToAssets(yieldVault.maxRedeem(address(this)));
931: uint256 _latentAssets = _asset.balanceOf(address(this));
936: yieldVault.redeem(_yieldVaultShares, address(this), address(this));
936: yieldVault.redeem(_yieldVaultShares, address(this), address(this));
938: if (_receiver != address(this)) {
```
[337](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L337) | [337](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L337) | [382](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L382) | [383](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L383) | [405](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L405) | [416](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L416) | [539](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L539) | [540](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L540) | [558](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L558) | [634](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L634) | [639](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L639) | [691](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L691) | [713](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L713) | [726](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L726) | [747](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L747) | [856](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L856) | [861](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L861) | [866](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L866) | [922](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L922) | [931](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L931) | [936](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L936) | [936](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L936) | [938](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L938)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

59: return twabController.balanceOf(address(this), _account);
64: return twabController.totalSupply(address(this));
```
[59](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L59) | [64](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L64)
</details>

### [G-13] Call `msg.sender` directly instead of caching it

In the instance below, instead of caching `msg.sender` and incurring unnecessary stack manipulation, we can call `msg.sender` directly.
```solidity
/// 3047 gas
function test() external {
    internalFunc(msg.sender);
    internalFunc(msg.sender);
    internalFunc(msg.sender);
}
// 3061 gas
function test() external {
    address callerLocal = msg.sender;
    internalFunc(callerLocal);
    internalFunc(callerLocal);
    internalFunc(callerLocal);
}
```

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

553: address _owner = msg.sender
```
[553](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L553)
</details>

### [G-14] Use `unchecked` for Non-Loop Increment/Decrement Operations

Disclaimer: `You should be sure that underflow is not possible `
Using `unchecked` increments can save gas by bypassing the built-in overflow checks.
This can save 30-40 gas per iteration.
It is recommended to use unchecked increments when overflow is not possible.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

103: deployerNonces[msg.sender]++
```
[103](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L103)
</details>

### [G-15] Constructors can be marked `payable`

Payable functions cost less gas to execute, since the compiler does not have to add extra checks to ensure that a payment wasn't provided.

A constructor can safely be marked as `payable`, since only the deployer would be able to pass funds, and the project itself would not pass any funds. 
T
his could save an average of about 21 gas per call, in addition to the extra deployment cost.

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
    ) TwabERC20(name_, symbol_, prizePool_.twabController()) Claimable(prizePool_, claimer_) Ownable(owner_) {
```
[289](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L289)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

42: constructor(
        string memory name_,
        string memory symbol_,
        TwabController twabController_
    ) ERC20(name_, symbol_) ERC20Permit(name_) {
```
[42](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L42)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

64: constructor(PrizePool prizePool_, address claimer_) {
```
[64](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L64)
</details>

### [G-16] Reduce gas usage by moving to Solidity 0.8.19 or later

See [this](https://soliditylang.org/blog/2023/02/22/solidity-0.8.19-release-announcement/#preventing-dead-code-in-runtime-bytecode) link for the full details. Additionally, every new release has new optimizations, which will save gas.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

2: pragma solidity ^0.8.0;
```
[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L2)
</details>

### [G-17] Reduce deployment costs by tweaking contracts' metadata

The Solidity compiler appends 53 bytes of metadata to the smart contract code which translates to an extra 10,600 gas (200 per bytecode) + the calldata cost (16 gas per non-zero bytes, 4 gas per zero-byte).
This translates to up to 848 additional gas in calldata cost.
One way to reduce this cost is by optimizing the IPFS hash that gets appended to the smart contract code.

Why is this important?
- The metadata adds an extra 53 bytes, resulting in an additional 10,600 gas cost for deployment.
- It also incurs up to 848 additional gas in calldata cost.

Options to Reduce Gas:
1. Use the `--no-cbor-metadata` compiler option to exclude metadata, but this might affect contract verification.
2. Mine for code comments that lead to an IPFS hash with more zeros, reducing calldata costs.

<details>
<summary><i>6 issue instances in 6 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

1: Consider optimizing the IPFS hash during deployment.
```
[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L1)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

1: Consider optimizing the IPFS hash during deployment.
```
[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L1)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

1: Consider optimizing the IPFS hash during deployment.
```
[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L1)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

1: Consider optimizing the IPFS hash during deployment.
```
[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L1)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

1: Consider optimizing the IPFS hash during deployment.
```
[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L1)

```solidity
File: pt-v5-vault/src/interfaces/IVaultHooks.sol

1: Consider optimizing the IPFS hash during deployment.
```
[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol#L1)
</details>

### [G-18] Use Pre-Increment/Decrement (++i/--i) to Save Gas

Using pre-increment (++i) or pre-decrement (--i) operators is more gas-efficient compared to their post counterparts (i++ or i--).
This is because pre-increment/decrement operators avoid the need for an additional temporary variable that stores the original value of the iterator.
This subtle difference results in saving of around 5 gas units per operation, which can accumulate to substantial savings in gas costs in contracts with frequent increment/decrement operations.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

103: salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))
```
[103](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L103)
</details>

### [G-19] Nesting `if`-statements is cheaper than using `&&`

Optimization of condition checks in your smart contract is a crucial aspect in ensuring gas efficiency. Specifically, substituting multiple `&&` checks with nested `if` statements can lead to substantial gas savings.

When evaluating multiple conditions within a single `if` statement using the `&&` operator, each condition will consume gas even if a preceding condition fails. However, if these checks are broken down into nested `if` statements, execution halts as soon as a condition fails, saving the gas that would have been consumed by subsequent checks.

This practice is especially beneficial in scenarios where the `if` statement isn't followed by an `else` statement. The reason being, when an `else` statement is present, all conditions must be checked regardless to determine the correct branch of execution.

By reworking your code to utilize nested `if` statements, you can optimize gas usage, reduce execution cost, and enhance your contract's performance.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

776: if (success && encodedDecimals.length >= 32) {
```
[776](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L776)
</details>

### [G-20] Avoid transferring amounts of zero in order to save gas

Performing token or Ether transfers with a zero amount may result in unnecessary gas consumption. 
The absence of a zero-amount check before a transfer or send operation can lead to wasted gas, as the state of the contract remains the same even if the amount is zero. 
Adding a conditional check for zero amounts can prevent these costly, unnecessary operations, thereby optimizing the contract's gas usage.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/TwabERC20.sol

/// @audit `SafeCast.toUint96(_amount)` has not been checked for zero value before transfer
101: twabController.transfer(_from, _to, SafeCast.toUint96(_amount));
```
[101](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L101)
</details>

### [G-21] `>=` costs less gas than `>`

The Solidity compiler requires fewer opcodes when the `>=` operator is used in place of the `>` operator. 
Specifically, the compiler uses GT and ISZERO opcodes for `>` but only requires LT for `>=`, saving 3 gas. 
Thus, wherever applicable, it's recommended to use `>=` instead of `>` to enhance gas efficiency in your code. Same applies for `<=` and `<`.

<details>
<summary><i>6 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

422: if (_ownerShares > _maxWithdraw) {
615: if (_shares > _yieldFeeBalance) revert SharesExceedsYieldFeeBalance(_shares, _yieldFeeBalance);
679: if (_amountOut + _yieldFee > _availableYield) {
874: if (totalAssets() < totalDebt()) revert LossyDeposit(totalAssets(), totalDebt());
932: if (_assets > _latentAssets) {
948: if (_yieldFeePercentage > MAX_YIELD_FEE) {
```
[422](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L422) | [615](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L615) | [679](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L679) | [874](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L874) | [932](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L932) | [948](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L948)
</details>

### [G-22] Use `unchecked` for Math Operations if they already checked

Some subtraction operations in the contract have implicit checks that prevent underflow. 
To optimize gas, consider wrapping such operations in an `unchecked` block. 
Always review the logic thoroughly before making changes to ensure the safety of operations.

<details>
<summary><i>5 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit - mathematical operation `_maxYieldVaultDeposit - _latentBalance` checked above in line:
/// 384: if (_latentBalance >= _maxYieldVaultDeposit) {
388: _maxDeposit = _maxYieldVaultDeposit - _latentBalance;
/// @audit - mathematical operation `_amountOut + _yieldFee` checked above in line:
/// 679: if (_amountOut + _yieldFee > _availableYield) {
680: revert LiquidationExceedsAvailable(_amountOut + _yieldFee, _availableYield);
/// @audit - mathematical operation `_totalAssets - totalDebt_` checked above in line:
/// 809: if (totalDebt_ >= _totalAssets) {
813: return _totalAssets - totalDebt_;
/// @audit - mathematical operation `totalYieldBalance_ - _yieldBuffer` checked above in line:
/// 826: if (totalYieldBalance_ >= _yieldBuffer) {
828: return totalYieldBalance_ - _yieldBuffer;
/// @audit - mathematical operation `_assets - _latentAssets` checked above in line:
/// 932: if (_assets > _latentAssets) {
934: uint256 _yieldVaultShares = yieldVault.previewWithdraw(_assets - _latentAssets);
```
[388](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L388) | [680](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L680) | [813](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L813) | [828](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L828) | [934](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L934)
</details>

### [G-23] Using `private` rather than `public`, saves gas

Public storage variables increase the contract's size due to the implicit generation of public getter functions. 
This makes the contract larger and could increase deployment and interaction costs.

If you do not require other contracts to read these variables, consider making them `private`. 

Example:
```solidity
/// 145426 gas to deploy
contract PublicState {
    address public first;
    address public second;
}
/// 77126 gas to deploy
contract PrivateState {
    address private first;
    address private second;
}
```

<details>
<summary><i>16 issue instances in 4 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

74: uint32 public constant FEE_PRECISION = 1e9;
80: uint32 public constant MAX_YIELD_FEE = 9e8;
112: uint256 public immutable yieldBuffer;
115: IERC4626 public immutable yieldVault;
119: uint32 public yieldFeePercentage;
122: address public yieldFeeRecipient;
125: uint256 public yieldFeeBalance;
128: address public liquidationPair;
```
[74](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L74) | [80](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L80) | [112](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L112) | [115](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L115) | [119](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L119) | [122](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L122) | [125](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L125) | [128](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L128)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

63: uint256 public constant YIELD_BUFFER = 1e5;
66: PrizeVault[] public allVaults;
69: mapping(address vault => bool deployedByFactory) public deployedVaults;
72: mapping(address deployer => uint256 nonce) public deployerNonces;
```
[63](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L63) | [66](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L66) | [69](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L69) | [72](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L72)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

26: TwabController public immutable twabController;
```
[26](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L26)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

21: uint24 public constant HOOK_GAS = 150_000;
24: PrizePool public immutable prizePool;
27: address public claimer;
```
[21](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L21) | [24](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L24) | [27](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L27)
</details>

### [G-24] Use Assembly for Hash Calculations

In certain cases, using inline assembly to calculate hashes can lead to significant gas savings. Solidity's built-in keccak256 function is convenient but costs more gas than the equivalent assembly code. However, it's important to note that using assembly should be done with care as it's less readable and could increase the risk of introducing errors.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

103: salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))
```
[103](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L103)
</details>

### [G-25] Refactor duplicated require()/revert() checks to save gas

Duplicate require()/revert() checks can be refactored into a modifier or function, saving deployment costs.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

612: if (_shares == 0) revert MintZeroShares();
844: if (_shares == 0) revert MintZeroShares();
```
[612](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L612) | [844](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L844)
</details>

### [G-26] Use Assembly for Efficient Memory Management in Multiple External Calls

When making multiple external calls in a Solidity contract, it may be more efficient to use inline assembly to reuse the memory space allocated for function arguments and return data.
This can prevent unnecessary memory expansion and reduce gas costs.

Example:
```solidity
contract Solidity {
    // cost: 7262
    function call(address calledAddress) external pure returns(uint256) {
        Called called = Called(calledAddress);
        uint256 res1 = called.add(1, 2);
        uint256 res2 = called.add(3, 4);

        uint256 res = res1 + res2;
        return res;
    }
}

contract Assembly {
    // cost: 5281
    function call(address calledAddress) external view returns(uint256) {
        assembly {
            // check that calledAddress has code deployed to it
            if iszero(extcodesize(calledAddress)) {
                revert(0x00, 0x00)
            }

            // first call
            mstore(0x00, hex"771602f7")
            mstore(0x04, 0x01)
            mstore(0x24, 0x02)
            let success := staticcall(gas(), calledAddress, 0x00, 0x44, 0x60, 0x20)
            if iszero(success) {
                revert(0x00, 0x00)
            }
            let res1 := mload(0x60)

            // second call
            mstore(0x04, 0x03)
            mstore(0x24, 0x4)
            success := staticcall(gas(), calledAddress, 0x00, 0x44, 0x60, 0x20)
            if iszero(success) {
                revert(0x00, 0x00)
            }
            let res2 := mload(0x60)

            // add results
            let res := add(res1, res2)

            // return data
            mstore(0x60, res)
            return(0x60, 0x20)
        }
    }
}
```

<details>
<summary><i>10 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit function `maxDeposit()` has multiple external calls
382: uint256 _latentBalance = _asset.balanceOf(address(this));
383: uint256 _maxYieldVaultDeposit = yieldVault.maxDeposit(address(this));
/// @audit function `_depositAndMint()` has multiple external calls
854: _asset.safeTransferFrom(
            _caller,
            address(this),
            _assets
        );
861: uint256 _assetsWithDust = _asset.balanceOf(address(this));
862: _asset.approve(address(yieldVault), _assetsWithDust);
865: uint256 _yieldVaultShares = yieldVault.previewDeposit(_assetsWithDust);
869: _asset.approve(address(yieldVault), 0);
/// @audit function `_withdraw()` has multiple external calls
931: uint256 _latentAssets = _asset.balanceOf(address(this));
934: uint256 _yieldVaultShares = yieldVault.previewWithdraw(_assets - _latentAssets);
936: yieldVault.redeem(_yieldVaultShares, address(this), address(this));
```
[382](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L382) | [383](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L383) | [854](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L854) | [861](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L861) | [862](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L862) | [865](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L865) | [869](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L869) | [931](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L931) | [934](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L934) | [936](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L936)
</details>

### [G-27] Cached Global Variables

Storing global variables in local storage might appear as a useful caching mechanism. 
However, in Solidity, accessing global variables directly is often more gas-efficient than caching them. 
Consider removing these redundant assignments and use the global variables directly.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

553: address _owner = msg.sender;
```
[553](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L553)
</details>

### [G-28] Simple checks for zero `uint` can be done using `assembly` to save gas

The usage of inline `assembly` to check if variable is the `zero` can save gas compared to traditional `require` or `if` statement checks.
```solidity
    // gas 396 gas | 399 gas
    assembly {
        if iszero(value) { // use iszero(iszero(value)) for != add 3 gas
            revert(0, 0)
        }
    }
    require(value != 0); // 401 gas
    require(value == 0); // 401 gas

<details>
<summary><i>8 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

458: if (_totalAssets == 0) revert ZeroTotalAssets();
612: if (_shares == 0) revert MintZeroShares();
665: if (_amountOut == 0) revert LiquidationAmountOutZero();
672: if (_yieldFeePercentage != 0) {
            // The yield fee is calculated as a portion of the total yield being consumed, such that 
            // `total = amountOut + yieldFee` and `yieldFee / total = yieldFeePercentage`. 
            _yieldFee = (_amountOut * FEE_PRECISION) / (FEE_PRECISION - _yieldFeePercentage) - _amountOut;
844: if (_shares == 0) revert MintZeroShares();
845: if (_assets == 0) revert DepositZeroAssets();
894: if (_assets == 0) revert WithdrawZeroAssets();
895: if (_shares == 0) revert BurnZeroShares();
```
[458](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L458) | [612](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L612) | [665](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L665) | [672](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L672) | [844](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L844) | [845](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L845) | [894](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L894) | [895](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L895)
</details>

### [G-29] Consider using solady's `FixedPointMathLib`

Utilizing gas-optimized math functions from libraries like [Solady](https://github.com/Vectorized/solady/blob/main/src/utils/FixedPointMathLib.sol) can lead to more efficient smart contracts.
This is particularly beneficial in contracts where these operations are frequently used.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

675: _yieldFee = (_amountOut * FEE_PRECISION) / (FEE_PRECISION - _yieldFeePercentage) - _amountOut;
```
[675](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L675)
</details>

### [G-30] Avoid Zero to Non-Zero Storage Writes Where Possible

Changing a storage variable from zero to non-zero costs 22,100 gas in total. (20,000 gas for a zero to non-zero write and 2,100 for a cold storage access)
Consider using non-zero architecture to avoid high gas costs for zero to non-zero storage writes.

Example:

```solidity
- uint256 public counter = 0;  // rewrite this costs more
+ uint256 public counter = 1;  // rewrite this costs less
```

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

617: yieldFeeBalance -= _yieldFeeBalance;
951: yieldFeePercentage = _yieldFeePercentage;
```
[617](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L617) | [951](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L951)
</details>

### [G-31] Use `unchecked` for division which do not divide by -X sonce they can't overflow

Solidity introduced the `unchecked` block in version 0.8.0 as a measure to provide control over arithmetic operations. 
Any operation inside this block will not trigger the built-in overflow and underflow checks, thus saving gas costs. 
Since a division operation between two `uint`s (unsigned integers) can never result in an overflow or underflow, it's an ideal candidate for the use of `unchecked {}` block.
This practice enables optimal gas usage without risking any arithmetic anomalies.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

675: _yieldFee = (_amountOut * FEE_PRECISION) / (FEE_PRECISION - _yieldFeePercentage) - _amountOut;
```
[675](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L675)
</details>

### [G-32] Use solady library where possible to save gas

The following OpenZeppelin imports have a Solady equivalent, as such they can be used to save GAS as Solady modules have been specifically designed to be as GAS efficient as possible.

<details>
<summary><i>6 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

3: import { IERC4626 } from "openzeppelin/interfaces/IERC4626.sol";
5: import { SafeERC20, IERC20Permit } from "openzeppelin/token/ERC20/utils/SafeERC20.sol";
6: import { ERC20, IERC20, IERC20Metadata } from "openzeppelin/token/ERC20/ERC20.sol";
```
[3](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L3) | [5](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L5) | [6](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L6)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

3: import { IERC20, IERC4626 } from "openzeppelin/token/ERC20/extensions/ERC4626.sol";
```
[3](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L3)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

3: import { ERC20 } from "openzeppelin/token/ERC20/ERC20.sol";
5: import { ERC20Permit } from "openzeppelin/token/ERC20/extensions/ERC20Permit.sol";
```
[3](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L3) | [5](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L5)
</details>

### [G-33] Use `uint256(1)`/`uint256(2)` instead of `true`/`false` to save gas for changes

Boolean variables in Solidity are more expensive than `uint256` or any type that takes up a full word, due to additional gas costs associated with write operations.
When using boolean variables, each write operation emits an extra SLOAD to read the slot's contents, replace the bits taken up by the boolean, and then write back.
This process cannot be disabled and leads to extra gas consumption.

By using `uint256(1)` and `uint256(2)` for representing true and false states, you can avoid a `Gwarmaccess` (100 gas) cost and also avoid a `Gsset` (20000 gas) cost when changing from `false` to `true`, after having been `true` in the past.
This approach helps in optimizing gas usage, making your contract more cost-effective.

[Usage in OpenZeppelin ReentrancyGuard.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27)

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

69: mapping(address vault => bool deployedByFactory) public deployedVaults;
```
[69](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L69)
</details>

### [G-34] Mark Functions That Revert For Normal Users As `payable`

Functions guaranteed to revert when called by normal users can be marked `payable`.
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function.
Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

<details>
<summary><i>8 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

611: function claimYieldFeeShares(uint256 _shares) external onlyYieldFeeRecipient {
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
735: function setClaimer(address _claimer) external onlyOwner {
742: function setLiquidationPair(address _liquidationPair) external onlyOwner {
753: function setYieldFeePercentage(uint32 _yieldFeePercentage) external onlyOwner {
759: function setYieldFeeRecipient(address _yieldFeeRecipient) external onlyOwner {
```
[611](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L611) | [659](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L659) | [703](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L703) | [735](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L735) | [742](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L742) | [753](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L753) | [759](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L759)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

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

### [G-35] Prefer `private` over `public` for Constants to Save Gas

Using `private` instead of `public` for constants can save gas.

The compiler doesn't need to create non-payable getter functions for deployment calldata, store the bytes of the value outside of where it's used, or add another entry to the method ID table, saving 3406-3606 gas in deployment.

If needed, the values can be read from the verified contract source code, or a single getter function that returns a tuple of the values of all currently public constants can be implemented.

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

### [G-36] `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

Using the addition operator instead of plus-equals saves gas.
Replace `<x> += <y>` or `<x> -= <y>` with `<x> = <x> + <y>` or `<x> = <x> - <y>`.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

617: yieldFeeBalance -= _yieldFeeBalance;
685: yieldFeeBalance += _yieldFee;
```
[617](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L617) | [685](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L685)
</details>

### [G-37] Using `bool`s for storage incurs overhead

Utilizing booleans for storage is less gas-efficient compared to using types that consume a full word like uint256.
Every write operation on a boolean necessitates an extra SLOAD operation to read the slot's current value, modify the boolean bits, and then write back.
This additional step is the compiler's measure against contract upgrades and pointer aliasing.

To enhance gas efficiency, consider using `uint256(0)` for false and `uint256(1)` for true, bypassing the extra Gwarmaccess (100 gas) incurred by the SLOAD.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

69: mapping(address vault => bool deployedByFactory) public deployedVaults;
```
[69](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L69)
</details>

### [G-38] Optimize Gas by Using Only Named Returns

The Solidity compiler can generate more efficient bytecode when using named returns.
It's recommended to replace anonymous returns with named returns for potential gas savings.

Example:
```solidity
/// 985 gas cost
function add(uint256 x, uint256 y) public pure returns (uint256) {
    return x + y;
}
/// 941 gas cost
function addNamed(uint256 x, uint256 y) public pure returns (uint256 res) {
    res = x + y;
}
```

<details>
<summary><i>40 issue instances in 6 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

320: function decimals() public view override(ERC20, IERC20Metadata) returns (uint8) {
329: function asset() external view returns (address) {
336: function totalAssets() public view returns (uint256) {
341: function convertToShares(uint256 _assets) public view returns (uint256) {
355: function convertToAssets(uint256 _shares) public view returns (uint256) {
374: function maxDeposit(address) public view returns (uint256) {
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
573: function totalDebt() public view returns (uint256) {
584: function totalYieldBalance() public view returns (uint256) {
591: function availableYieldBalance() public view returns (uint256) {
597: function currentYieldBuffer() external view returns (uint256) {
631: function liquidatableBalanceOf(address _tokenOut) public view returns (uint256) {
659: function transferTokensOut(
        address,
        address _receiver,
        address _tokenOut,
        uint256 _amountOut
    ) public virtual onlyLiquidationPair returns (bytes memory) {
717: function targetOf(address) external view returns (address) {
722: function isLiquidationPair(
        address _tokenOut,
        address _liquidationPair
    ) external view returns (bool) {
772: function _tryGetAssetDecimals(IERC20 asset_) internal view returns (bool, uint8) {
790: function _totalDebt(uint256 _totalSupply) internal view returns (uint256) {
798: function _twabSupplyLimit(uint256 _totalSupply) internal pure returns (uint256) {
808: function _totalYieldBalance(uint256 _totalAssets, uint256 totalDebt_) internal pure returns (uint256) {
823: function _availableYieldBalance(uint256 _totalAssets, uint256 totalDebt_) internal view returns (uint256) {
921: function _maxYieldVaultWithdraw() internal view returns (uint256) {
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
    ) external returns (PrizeVault) {
136: function totalVaults() external view returns (uint256) {
```
[92](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L92) | [136](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L136)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

56: function balanceOf(
        address _account
    ) public view virtual override(ERC20) returns (uint256) {
63: function totalSupply() public view virtual override(ERC20) returns (uint256) {
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
    ) external onlyClaimer returns (uint256) {
```
[76](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L76)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

22: function getHooks(address account) external view returns (VaultHooks memory) {
```
[22](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L22)

```solidity
File: pt-v5-vault/src/interfaces/IVaultHooks.sol

26: function beforeClaimPrize(
        address winner,
        uint8 tier,
        uint32 prizeIndex,
        uint96 reward,
        address rewardRecipient
    ) external returns (address);
```
[26](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol#L26)
</details>

### [G-39] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead. The Ethereum Virtual Machine (EVM) operates on 32 bytes at a time. Therefore, if an element is smaller than 32 bytes, the EVM must use more operations to reduce the size of the element from 32 bytes to the desired size. 

Operations involving smaller size uints/ints cost extra gas due to the compiler having to clear the higher bits of the memory word before operating on the small size integer. This also includes the associated stack operations of doing so. 

It's recommended to use larger sizes and downcast where needed to optimize for gas efficiency.

<details>
<summary><i>19 issue instances in 4 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

74: uint32 public constant FEE_PRECISION = 1e9;
80: uint32 public constant MAX_YIELD_FEE = 9e8;
119: uint32 public yieldFeePercentage;
138: uint8 private immutable _underlyingDecimals;
296: uint32 yieldFeePercentage_,
        uint256 yieldBuffer_,
        address owner_
    ) TwabERC20(name_, symbol_, prizePool_.twabController()) Claimable(prizePool_, claimer_) Ownable(owner_) {
304: (bool success, uint8 assetDecimals) = _tryGetAssetDecimals(asset_);
320: function decimals() public view override(ERC20, IERC20Metadata) returns (uint8) {
668: uint32 _yieldFeePercentage = yieldFeePercentage;
753: function setYieldFeePercentage(uint32 _yieldFeePercentage) external onlyOwner {
772: function _tryGetAssetDecimals(IERC20 asset_) internal view returns (bool, uint8) {
778: if (returnedDecimals <= type(uint8).max) {
779: return (true, uint8(returnedDecimals));
800: return type(uint96).max - _totalSupply;
947: function _setYieldFeePercentage(uint32 _yieldFeePercentage) internal {
```
[74](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L74) | [80](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L80) | [119](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L119) | [138](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L138) | [296](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L296) | [304](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L304) | [320](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L320) | [668](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L668) | [753](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L753) | [772](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L772) | [778](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L778) | [779](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L779) | [800](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L800) | [947](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L947)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

99: uint32 _yieldFeePercentage,
      address _owner
    ) external returns (PrizeVault) {
```
[99](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L99)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

21: uint24 public constant HOOK_GAS = 150_000;
78: uint8 _tier,
        uint32 _prizeIndex,
        uint96 _reward,
        address _rewardRecipient
    ) external onlyClaimer returns (uint256) {
```
[21](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L21) | [78](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L78)

```solidity
File: pt-v5-vault/src/interfaces/IVaultHooks.sol

28: uint8 tier,
        uint32 prizeIndex,
        uint96 reward,
        address rewardRecipient
    ) external returns (address);
42: uint8 tier,
        uint32 prizeIndex,
        uint256 prize,
        address recipient
    ) external;
```
[28](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol#L28) | [42](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol#L42)
</details>

### [G-40] Use assembly to check for `address(0)`

The usage of inline assembly to check if variable is the zero can save gas compared to traditional `require` or `if` statement checks. 

The assembly check uses the `extcodesize` operation which is generally cheaper in terms of gas.

[More information can be found here.](https://medium.com/@kalexotsu/solidity-assembly-checking-if-an-address-is-0-efficiently-d2bfe071331)

<details>
<summary><i>7 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

300: if (address(yieldVault_) == address(0)) revert YieldVaultZeroAddress();
301: if (owner_ == address(0)) revert OwnerZeroAddress();
743: if (address(_liquidationPair) == address(0)) revert LPZeroAddress();
```
[300](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L300) | [301](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L301) | [743](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L743)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

47: if (address(0) == address(twabController_)) revert TwabControllerZeroAddress();
```
[47](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L47)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

65: if (address(prizePool_) == address(0)) revert PrizePoolZeroAddress();
97: if (recipient == address(0)) revert ClaimRecipientZeroAddress();
129: if (_claimer == address(0)) revert ClaimerZeroAddress();
```
[65](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L65) | [97](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L97) | [129](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L129)
</details>

### [G-41] `internal`/`private` functions only called once can be inlined to save gas

`internal` functions that are only called once should be inlined to save gas. 
Not inlining such functions costs an extra 20 to 40 gas due to the additional `JUMP` instructions and stack operations required for function calls.

<details>
<summary><i>2 issue instances in 2 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

/// @audit function `_tryGetAssetDecimals()` called only once
772: function _tryGetAssetDecimals(IERC20 asset_) internal view returns (bool, uint8) {
```
[772](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L772)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

/// @audit function `_setClaimer()` called only once
128: function _setClaimer(address _claimer) internal {
```
[128](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L128)
</details>

### [G-42] Declare `immutable` as `private` to save gas

Using `private` instead of `public` for immutables saves gas.

The compiler doesn't need to create non-payable getter functions for deployment calldata, store the bytes of the value outside of where it's used, or add another entry to the method ID table, saving 3406-3606 gas in deployment.

<details>
<summary><i>4 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

112: uint256 public immutable yieldBuffer;
115: IERC4626 public immutable yieldVault;
```
[112](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L112) | [115](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L115)

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

### [G-43] Optimize names to save gas

Function names for public and external methods, as well as public state variable names, can be optimized to achieve gas savings.
By renaming functions to generate method IDs with two leading zero bytes, you can save 128 gas during contract deployment.
Further, renaming functions for lower method IDs can conserve 22 gas per call for each sorted position shifted.

Optimizing function names for gas efficiency can result in significant savings, especially for frequently called functions or heavily deployed contracts. 
Reference: [Solidity Gas Optimizations - Function Name](https://blog.emn178.cc/en/post/solidity-gas-optimization-function-name/)

<details>
<summary><i>5 issue instances in 5 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

65: contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownable {
```
[65](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L65)

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol

13: contract PrizeVaultFactory {
```
[13](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L13)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

19: contract TwabERC20 is ERC20, ERC20Permit {
```
[19](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L19)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

13: abstract contract Claimable is HookManager, IClaimable {
```
[13](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L13)

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol

9: abstract contract HookManager {
```
[9](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L9)
</details>

### [G-44] Avoid updating storage when the value hasn't changed

A check regarding whether the current value and the new value are the same should be added.
This helps prevent unnecessary state changes and events in case the new value is the same as the current value.

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

### [G-45] Optimize External Calls with Assembly for Memory Efficiency

Using interfaces to make external contract calls in Solidity is convenient but can be inefficient in terms of memory utilization.
Each such call involves creating a new memory location to store the data being passed, thus incurring memory expansion costs. 

Inline assembly allows for optimized memory usage by re-using already allocated memory spaces or using the scratch space for smaller datasets.
This can result in notable gas savings, especially for contracts that make frequent external calls.

Additionally, using inline assembly enables important safety checks like verifying if the target address has code deployed to it using `extcodesize(addr)` before making the call, mitigating risks associated with contract interactions.

<details>
<summary><i>24 issue instances in 3 files:</i></summary>

```solidity
File: pt-v5-vault/src/PrizeVault.sol

337: return yieldVault.convertToAssets(yieldVault.balanceOf(address(this))) + _asset.balanceOf(address(this));
382: uint256 _latentBalance = _asset.balanceOf(address(this));
383: uint256 _maxYieldVaultDeposit = yieldVault.maxDeposit(address(this));
405: uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));
416: uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));
539: if (_asset.allowance(_owner, address(this)) != _assets) {
            IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);
639: _maxAmountOut = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));
854: _asset.safeTransferFrom(
            _caller,
            address(this),
            _assets
        );
861: uint256 _assetsWithDust = _asset.balanceOf(address(this));
862: _asset.approve(address(yieldVault), _assetsWithDust);
865: uint256 _yieldVaultShares = yieldVault.previewDeposit(_assetsWithDust);
866: uint256 _assetsUsed = yieldVault.mint(_yieldVaultShares, address(this));
869: _asset.approve(address(yieldVault), 0);
922: return yieldVault.convertToAssets(yieldVault.maxRedeem(address(this)));
931: uint256 _latentAssets = _asset.balanceOf(address(this));
934: uint256 _yieldVaultShares = yieldVault.previewWithdraw(_assets - _latentAssets);
936: yieldVault.redeem(_yieldVaultShares, address(this), address(this));
939: _asset.transfer(_receiver, _assets);
```
[337](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L337) | [382](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L382) | [383](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L383) | [405](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L405) | [416](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L416) | [539](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L539) | [639](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L639) | [854](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L854) | [861](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L861) | [862](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L862) | [865](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L865) | [866](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L866) | [869](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L869) | [922](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L922) | [931](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L931) | [934](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L934) | [936](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L936) | [939](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L939)

```solidity
File: pt-v5-vault/src/TwabERC20.sol

59: return twabController.balanceOf(address(this), _account);
64: return twabController.totalSupply(address(this));
77: twabController.mint(_receiver, SafeCast.toUint96(_amount));
88: twabController.burn(_owner, SafeCast.toUint96(_amount));
101: twabController.transfer(_from, _to, SafeCast.toUint96(_amount));
```
[59](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L59) | [64](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L64) | [77](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L77) | [88](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L88) | [101](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L101)

```solidity
File: pt-v5-vault/src/abstract/Claimable.sol

99: uint256 prizeTotal = prizePool.claimPrize(
            _winner,
            _tier,
            _prizeIndex,
            recipient,
            _reward,
            _rewardRecipient
        );
```
[99](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L99)
</details>

