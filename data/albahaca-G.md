# GAS OPTIMIZATION for PoolTogether

# SUMMARY
|      |  issue  |  instance  |
|------|---------|------------|
|[G‑01]|Use assembly to write address storage values|3|
|[G‑02]|Avoid contract existence checks by using low level calls|1|
|[G‑03]|Use Assembly To Check For address(0)|7|
|[G‑04]|State variables can be packed to use fewer storage slots|1|
|[G‑05]|use Mappings Instead of Arrays|1|
|[G‑06]|Gas Optimization Issue - Use Constants Instead of type(uintx).max|2|
|[G‑07]|Multiple Address/id Mappings Can Be Combined Into A Single Mapping Of An Address/id To A Struct, Where Appropriate|2|
|[G‑08]|Gas Optimization Issue - Gas-Efficient Alternatives for Common Math Operations|5|
|[G‑09]|Use assembly to perform efficient back-to-back calls|2|
|[G‑10]|Caching global variables is more expensive than using the actual variable (use msg.sender instead of caching it)|1|
|[G‑11]|Use assembly in place of abi.decode to extract calldata values more efficiently|1|
|[G‑12]|Optimize External Calls with Assembly for Memory Efficiency|1|
|[G‑13]|Ternary operation is cheaper than if-else statement|1|
|[G‑14]|A modifier used only once and not being inherited should be inlined to save gas|2|
|[G‑15]|Make 3 event parameters indexed when possible|3|


## [G-01] Use assembly to write address storage values

By using assembly to write to address storage values, you can bypass some of these operations and lower the gas cost of writing to storage. Assembly code allows you to directly access the Ethereum Virtual Machine (EVM) and perform low-level operations that are not possible in Solidity.

example of using assembly to write to address storage values:
```
contract MyContract {
    address private myAddress;
    
    function setAddressUsingAssembly(address newAddress) public {
        assembly {
            sstore(0, newAddress)
        }
    }
}
```

🔴 `Affected code:`
There are 3 instances of this issue:
```solidity
File:  PrizeVault.sol
745   liquidationPair = _liquidationPair;

959   yieldFeeRecipient = _yieldFeeRecipient;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L745

```solidity
File:  abstract/Claimable.sol
130   claimer = _claimer;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L130

## [G‑02] Avoid contract existence checks by using low level calls
the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence.

🔴 `Affected code:`
There are 1 instances of this issue:
```solidity
File:  PrizeVaultFactory.sol
118  IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L118



## [G-03] Use Assembly To Check For address(0)
Saves 7 gas per instance if using assembly to check for address(0)
PrizeVault.sol
Note: thes are missed from bots

🔴 `Affected code:`
There are 7 instances of this issue:
```solidity
File:  PrizeVault.sol
300   if (address(yieldVault_) == address(0)) revert YieldVaultZeroAddress();

301  if (owner_ == address(0)) revert OwnerZeroAddress();

743  if (address(_liquidationPair) == address(0)) revert LPZeroAddress();
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L300


```solidity
File:  TwabERC20.sol
47  if (address(0) == address(twabController_)) revert TwabControllerZeroAddress();
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L47


```solidity
File:  abstract/Claimable.sol
65  if (address(prizePool_) == address(0)) revert PrizePoolZeroAddress();

97  if (recipient == address(0)) revert ClaimRecipientZeroAddress();

129 if (_claimer == address(0)) revert ClaimerZeroAddress();
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L65


## [G-04] State variables can be packed to use fewer storage slots
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if the values combined are <= 32 bytes). If the variables packed together are retrieved together in functions we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a Gwarmaccess (100 gas) versus a Gcoldsload (2100 gas).

🔴 `Affected code:`
There are 1 instances of this issue:

- This use two storage slots
```solidity
File:  PrizeVault.sol
    uint256 public yieldFeeBalance;

    /// @notice Address of the liquidation pair used to liquidate yield for prize token.
    address public liquidationPair;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L125-L128


**`Matigation`**
```diff
File:  PrizeVault.sol
-   uint256 public yieldFeeBalance;
+   uint64 public yieldFeeBalance;

    /// @notice Address of the liquidation pair used to liquidate yield for prize token.
    address public liquidationPair;
```

When two variables are packed next to each other in storage and their combined size is less than or equal to 32 bytes, they will indeed occupy the same storage slot. Let's reassess:

1. **yieldFeeBalance** (`uint64`):
   - Size: 8 bytes

2. **liquidationPair** (`address`):
   - Size: 20 bytes

The combined size is 28 bytes, which is less than the capacity of a single storage slot (32 bytes). Therefore, both variables can be packed together into a single storage slot, optimizing storage usage.

So, in this scenario:

- Both `yieldFeeBalance` and `liquidationPair` can indeed share the same storage slot, requiring only one storage slot in total.  	


## [G-05]use Mappings Instead of Arrays
Arrays are useful when you need to maintain an ordered list of data that can be iterated over, but they have a higher gas cost for read and write operations, especially when the size of the array is large. This is because Solidity needs to iterate over the entire array to perform certain operations, such as finding a specific element or deleting an element.

Mappings, on the other hand, are useful when you need to store and access data based on a key, rather than an index. Mappings have a lower gas cost for read and write operations, especially when the size of the mapping is large, since Solidity can perform these operations based on the key directly, without needing to iterate over the entire data structure.

🔴 `Affected code:`
There are 1 instances of this issue:
```solidity
File:  PrizeVaultFactory.sol
66  PrizeVault[] public allVaults;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L66


## [G-06] Gas Optimization Issue - Use Constants Instead of type(uintx).max

**Explanation of Issue:**
Utilizing constants instead of `type(uintx).max` is essential for gas optimization in Ethereum smart contracts. Constants, being precomputed at compile-time, offer a more efficient approach compared to the runtime computations required by `type(uintx).max`, resulting in significant gas savings.

**Examples of Code:**

- **Non-Optimized:**
```solidity
function transferFundsNonOptimized(address _to, uint256 _amount) public {
    require(balanceOf(msg.sender) >= _amount, "Insufficient balance");
    require(_amount < type(uint256).max, "Amount exceeds maximum value");
    
    // Transfer logic
    // ...
}
```

- **Optimized:**
```solidity
uint256 constant MAX_UINT256 = type(uint256).max;

function transferFundsOptimized(address _to, uint256 _amount) public {
    require(balanceOf(msg.sender) >= _amount, "Insufficient balance");
    require(_amount < MAX_UINT256, "Amount exceeds maximum value");

    // Transfer logic
    // ...
}
```

**Reason:**
In the non-optimized example, using `type(uint256).max` within the function introduces unnecessary runtime computations, leading to higher gas consumption. The optimized version, using a constant (`MAX_UINT256`), ensures that the maximum value is precomputed at compile-time, resulting in a more gas-efficient smart contract. This optimization becomes crucial for contracts that involve frequent arithmetic operations with the maximum possible value.
🔴 `Affected code:`
There are 2 instances of this issue:
```solidity
File:  PrizeVaultFactory.sol
778   if (returnedDecimals <= type(uint8).max) {

800   return type(uint96).max - _totalSupply;    
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L778


## [G-07] Multiple Address/id Mappings Can Be Combined Into A Single Mapping Of An Address/id To A Struct, Where Appropriate
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to [not having to recalculate the key’s keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.
🔴 `Affected code:`
There are 1 instances of this issue:
```solidity
File:  PrizeVaultFactory.sol
    mapping(address vault => bool deployedByFactory) public deployedVaults;

    /// @notice Mapping to store deployer nonces for CREATE2
    mapping(address deployer => uint256 nonce) public deployerNonces;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L69-L72


## [G-08] Gas Optimization Issue - Gas-Efficient Alternatives for Common Math Operations

**Explanation of Issue:**
Common math operations, such as min and max, may have more gas-efficient alternatives. In scenarios where unoptimized code uses conditional operators, like the ternary operator, the resulting conditional jumps in opcodes can incur higher gas costs. Exploring gas-efficient alternatives for these operations is crucial for optimizing smart contract performance.

**Examples of Code:**
*Non-optimized Code:*
```solidity
function max(uint256 x, uint256 y) public pure returns (uint256 z) {
    z = x > y ? x : y; // Unoptimized conditional operator
}
```

*Optimized Code:*
```solidity
function max(uint256 x, uint256 y) public pure returns (uint256 z) {
    /// @solidity memory-safe-assembly
    assembly {
        z := xor(x, mul(xor(x, y), gt(y, x))) // Gas-efficient alternative
    }
}
```

**Reason:**
In the non-optimized example, the ternary operator is used for the max function, introducing conditional jumps in the opcodes, which can be gas-costly. The optimized code provides a gas-efficient alternative using assembly code. By avoiding conditional operators, the gas consumption is reduced, leading to improved contract efficiency. The Solady Library offers additional gas-efficient math operations, making it valuable for developers seeking optimization opportunities.

🔴 `Affected code:`
There are 5 instances of this issue:
```solidity
File:  PrizeVault.sol
305  _underlyingDecimals = success ? assetDecimals : 18;

390  return twabSupplyLimit_ < _maxDeposit ? twabSupplyLimit_ : _maxDeposit;

490  return _ownerAssets < _maxWithdraw ? _ownerAssets : _maxWithdraw;

433  return _maxScaledRedeem >= _ownerShares ? _ownerShares : _maxScaledRedeem;

653  return _liquidYield >= _maxAmountOut ? _maxAmountOut : _liquidYield;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L305

## [G-09] Use assembly to perform efficient back-to-back calls
If a similar external call is performed back-to-back, we can use assembly to reuse any function signatures and function parameters that stay the same. In addition, we can also reuse the same memory space for each function call (scratch space + free memory pointer + zero slot), which can potentially allow us to avoid memory expansion costs.
Note: In order to do this optimization safely we will cache the free memory pointer value and restore it once we are done with our function calls. We will also set the zero slot back to 0 if neccessary.
[Reffrence](https://code4rena.com/reports/2023-05-ajna#g-10-use-assembly-to-perform-efficient-back-to-back-calls)
🔴 `Affected code:`
There are 2 instances of this issue:
```solidity
File:  PrizeVault.sol
861        uint256 _assetsWithDust = _asset.balanceOf(address(this));
862        _asset.approve(address(yieldVault), _assetsWithDust);

865        uint256 _yieldVaultShares = yieldVault.previewDeposit(_assetsWithDust);
866        uint256 _assetsUsed = yieldVault.mint(_yieldVaultShares, address(this));
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L861


## [G-10] Caching global variables is more expensive than using the actual variable (use msg.sender instead of caching it)

🔴 `Affected code:`
There are 1 instances of this issue:
```solidity
File:  PrizeVault.sol
553  address _owner = msg.sender;
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L553


## [G-11] Use assembly in place of abi.decode to extract calldata values more efficiently
Instead of using abi.decode, we can use assembly to decode our desired calldata values directly. This will allow us to avoid decoding calldata values that we will not use.
[Reffrence](https://code4rena.com/reports/2023-05-juicebox#g-04-use-assembly-in-place-of-abidecode-to-extract-calldata-values-more-efficiently)

🔴 `Affected code:`
There are 1 instances of this issue:
```solidity
File:  PrizeVault.sol
777  uint256 returnedDecimals = abi.decode(encodedDecimals, (uint256));
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L777

## [G-12] Optimize External Calls with Assembly for Memory Efficiency
Using interfaces to make external contract calls in Solidity is convenient but can be inefficient in terms of memory utilization. Each such call involves creating a new memory location to store the data being passed, thus incurring memory expansion costs.
Inline assembly allows for optimized memory usage by re-using already allocated memory spaces or using the scratch space for smaller datasets. This can result in notable gas savings, especially for contracts that make frequent external calls.
Additionally, using inline assembly enables important safety checks like verifying if the target address has code deployed to it using extcodesize(addr) before making the call, mitigating risks associated with contract interactions.
[Reffrence](https://github.com/code-423n4/2023-10-ethena/blob/main/bot-report.md#g-10-optimize-external-calls-with-assembly-for-memory-efficiency)

🔴 `Affected code:`
There are 1 instances of this issue:
```solidity
File:  PrizeVault.sol
936   yieldVault.redeem(_yieldVaultShares, address(this), address(this));
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L936



## [G-13] Ternary operation is cheaper than if-else statement

🔴 `Affected code:`
There are 1 instances of this issue:
```solidity
File:  PrizeVault.sol
        if (totalYieldBalance_ >= _yieldBuffer) {
            return _yieldBuffer;
        } else {
            return totalYieldBalance_;
        }
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L600-L604

## [G-14] A modifier used only once and not being inherited should be inlined to save gas
When you use a modifier in Solidity, Solidity generates code to check the conditions of the modifier and execute the modified function if the conditions are met. This generated code can consume gas, especially if the modifier is used frequently or if the modified function is called multiple times.
By inlining a modifier that is used only once and not being inherited, you can eliminate the overhead of the generated code and reduce the gas cost of your contract.

🔴 `Affected code:`
There are 2 instances of this issue:

- the `onlyClaimer` modifier only used once in `Claimable::claimPrize`
```solidity
File:  abstract/Claimable.sol
    modifier onlyClaimer() {
        if (msg.sender != claimer) revert CallerNotClaimer(msg.sender, claimer);
        _;
    }
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L52-L55

- the `onlyYieldFeeRecipient` modifier only used once in `PrizeVault::claimYieldFeeShares`
```solidity
File:  PrizeVault.sol
    modifier onlyYieldFeeRecipient() {
        if (msg.sender != yieldFeeRecipient) {
            revert CallerNotYieldFeeRecipient(msg.sender, yieldFeeRecipient);
        }
        _;
    }
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L267-L273

## [G-15] Make 3 event parameters indexed when possible
It is the most gas efficient to make up to 3 event parameters indexed. If there are less than 3 parameters, you need to make all parameters indexed.

🔴 `Affected code:`
There are 3 instances of this issue:

```solidity
File:  PrizeVault.sol
150  event YieldFeePercentageSet(uint256 yieldFeePercentage);

156   event Sponsor(address indexed caller, uint256 assets, uint256 shares);

175   event ClaimYieldFeeShares(address indexed recipient, uint256 shares);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L150


