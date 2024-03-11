

# Gas optimization

| Number | Issue | Instences |
|--------|-------|-----------|
|[G-01]| State variables can be packed into fewer storage slots | 1 |
|[G-02]| Mappings not used externally/internally can be marked private | 2 |
|[G-03]| Low level call can be optimized with assembly | 3 |
|[G-04]| Precompute Off-Chain When Possible | 1 |
|[G-05]| Using bytes32 is cheaper than using string.  | 8 |
|[G-06]| Check Arguments Early | 2 |
|[G-07]| State variables that could be declared constant | 6 |


## [G-01] State variables can be packed into fewer storage slots

```solidity
file:  blob/main/pt-v5-vault/src/PrizeVault.sol
 
119     uint32 public yieldFeePercentage;

         /// @notice Address of the yield fee recipient.
         address public yieldFeeRecipient;
     
         /// @notice The accrued yield fee balance that the fee recipient can claim as vault shares.
         uint256 public yieldFeeBalance;
     
         /// @notice Address of the liquidation pair used to liquidate yield for prize token.
         address public liquidationPair;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L119-L128

## [G-02] Mappings not used externally/internally can be marked private

The below mappings from the PrizeVaultFactory.sol contract can be marked private since they’re not used by any other contracts.

```solidity
file: blob/main/pt-v5-vault/src/PrizeVaultFactory.sol

69    mapping(address vault => bool deployedByFactory) public deployedVaults;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L69

```solidity
file: blob/main/pt-v5-vault/src/PrizeVaultFactory.sol

72    mapping(address deployer => uint256 nonce) public deployerNonces;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L72


## [G-03] Low level call can be optimized with assembly

returnData is copied to memory even if the variable is not utilized: the proper way to handle this is through a low level assembly call.

<details> 

```solidity
// before
(bool success,) = payable(receiver).call{gas: gas, value: value}("");

//after
bool success;
assembly {
    success := call(gas, receiver, value, 0, 0, 0, 0)
} 
```
</details>

```solidity
file: blob/main/pt-v5-vault/src/PrizeVault.sol

540  IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);

773   (bool success, bytes memory encodedDecimals) = address(asset_).staticcall(
            abi.encodeWithSelector(IERC20Metadata.decimals.selector)
        );
```

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L540


```solidity
file: blob/main/pt-v5-vault/src/PrizeVaultFactory.sol

118    IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER);

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L118

## [G-04] Precompute Off-Chain When Possible

Computing values in advance outside the blockchain is cheaper than doing it inside smart contracts:

```solidity
// On-chain computation
function verify(uint numA, uint numB) {
  require(numA * numB < 1000);
}

// Precomputed off-chain
function verify(uint result) {
  require(result < 1000); 
}
```

Do computations like hashes, signatures outside.
Pass in precomputed values to save on-chain gas.

```solidity
file: blob/main/pt-v5-vault/src/PrizeVault.sol

679   if (_amountOut + _yieldFee > _availableYield) {

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L69

## [G-05] Using bytes32 is cheaper than using string. 

Storing and manipulating data in bytes32 is more gas-efficient than using string, which involves dynamic storage allocation. bytes32 is a fixed-size type, whereas string can have variable length, resulting in higher gas costs.

```solidity
file: blob/main/pt-v5-vault/src/PrizeVault.sol

290  string memory name_,

291  string memory symbol_,

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L290


```solidity
file: blob/main/pt-v5-vault/src/PrizeVaultFactory.sol

29   string name,

30   string symbol

93   string memory _name,

94   string memory _symbol,

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L29

```solidity
file: blob/main/pt-v5-vault/src/TwabERC20.sol

43   string memory name_,

44   string memory symbol_,

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L43


## [G-06] Check Arguments Early

Checks that require() or if() statements that check input arguments are at the top of the function. Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to if before wasting a gas load in a function that may ultimately if in the unhappy case.


```solidity
file: blob/main/pt-v5-vault/src/PrizeVault.sol

679   if (_amountOut + _yieldFee > _availableYield) {
            revert LiquidationExceedsAvailable(_amountOut + _yieldFee, _availableYield);
        }

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L679-L681

```solidity
file: blob/main/pt-v5-vault/src/PrizeVault.sol

634   if (_tokenOut == address(this)) {
            // Liquidation of vault shares is capped to the TWAB supply limit.
            _maxAmountOut = _twabSupplyLimit(_totalSupply);
        } else if (_tokenOut == address(_asset)) {
            // Liquidation of yield assets is capped at the max yield vault withdraw plus any latent balance.
            _maxAmountOut = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));
        } else {
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L634-L640

## [G-07] State variables that could be declared constant

State variables that are not updated following deployment should be declared constant to save gas.

```solidity
file: blob/main/pt-v5-vault/src/PrizeVault.sol

119   uint32 public yieldFeePercentage;

122   address public yieldFeeRecipient;

125   uint256 public yieldFeeBalance;

128   address public liquidationPair;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L119

```solidity
file: blob/main/pt-v5-vault/src/PrizeVaultFactory.sol

66  PrizeVault[] public allVaults;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L66


```solidity
file: blob/main/pt-v5-vault/src/abstract/Claimable.sol

27   address public claimer;

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L27