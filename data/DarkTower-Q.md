## [L-01] No support for ERC4626 vaults above or below a 1:1 mint ratio
Not all ERC4626 vaults follow a 1:1 asset to share mint ratio. While the `PrizeVault` tries to follow a 1:1 mint ratio, other vaults it is integrating with may not follow such ratio. For example if a vault on Aave mints 1 share for 100 USDC, and the `PrizeVault` mints 1 share for USDC, such deposits into the strategy yield vault would round to 0.

- (PrizeVault.sol#L476-L479)[https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L476-L479]
- (PrizeVault.sol#L482-L486)[https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L482-L486]

```js
  FILE: /pt-v5-vault/PrizeVault.sol
  function deposit(uint256 _assets, address _receiver) external returns (uint256) {
      uint256 _shares = previewDeposit(_assets); // @audit could just query the yield vault of this prize vault integeration directly and enforce a min amount to recieve so the caller knows what to expect
      _depositAndMint(msg.sender, _receiver, _assets, _shares);
      return _shares;
  }

function mint(uint256 _shares, address _receiver) external returns (uint256) {
    uint256 _assets = previewMint(_shares);
    _depositAndMint(msg.sender, _receiver, _assets, _shares);
    return _assets;
}
```

Since the user's deposit is sent into the `yieldVault` directly, previewing the deposit on the `yieldVault` directly would be a better source of truth for how much shares will be minted to the share and sets up an expectation that they can see and potentially accept it's result:

```diff
function deposit(uint256 _assets, address _receiver) external returns (uint256) {
-  uint256 _shares = previewDeposit(_assets);
+  uint256 _shares = yieldVault.previewDeposit(_assets); 
  
  _depositAndMint(msg.sender, _receiver, _assets, _shares);
  return _shares;
}
```

```diff
function mint(uint256 _shares, address _receiver) external returns (uint256) {
-  uint256 _assets = previewMint(_shares);
+  uint256 _assets = yieldVault.previewMint(_shares);

  _depositAndMint(msg.sender, _receiver, _assets, _shares);
  return _assets;
}
```

## [L-02] `convertToShares` visibility should be external
The `convertToShares` function is not being used anywhere in the `PrizeVault` contract. It's visibility can be set to external.

- (PrizeVault.sol#L341)[https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L341]

```diff
+ function convertToShares(uint256 _assets) external view returns (uint256) {
- function convertToShares(uint256 _assets) public view returns (uint256) {
    uint256 totalDebt_ = totalDebt();
    uint256 _totalAssets = totalAssets();
    if (_totalAssets >= totalDebt_) {
        return _assets;
    } else {
        // If the vault controls less assets than what has been deposited a share will be worth a
        // proportional amount of the total assets. This can happen due to fees, slippage, or loss
        // of funds in the underlying yield vault.
        return _assets.mulDiv(totalDebt_, _totalAssets, Math.Rounding.Down);
    }
}
```

## [L-03] Prize vault deployments can be engineered to fool users
Deployments of prize vaults take in an owner parameter but this is not sufficient when there can be 5 vaults at a time with the same `name`, `symbol`, `_yieldVault` etc but their address being different. It can create a scenario where if users are depositing for the first time, they can't figure out which one they were supposed to deposit into leading to a scenario where they deposit in other similar vaults.

- (PrizeVaultFactory.sol#L92-L132)[https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L92-L132]

```solidity
function deployVault(
      string memory _name,
      string memory _symbol,
      IERC4626 _yieldVault,
      PrizePool _prizePool,
      address _claimer,
      address _yieldFeeRecipient,
      uint32 _yieldFeePercentage,
      address _owner
    ) external returns (PrizeVault) {
        PrizeVault _vault = new PrizeVault{
            salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))
        }(
            _name,
            _symbol,
            _yieldVault,
            _prizePool,
            _claimer,
            _yieldFeeRecipient,
            _yieldFeePercentage,
            YIELD_BUFFER,
            _owner
        );

        // A donation to fill the yield buffer is made to ensure that early depositors have
        // rounding errors covered in the time before yield is actually generated.
        IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER);

        allVaults.push(_vault);
        deployedVaults[address(_vault)] = true;

        emit NewPrizeVault(
            _vault,
            _yieldVault,
            _prizePool,
            _name,
            _symbol
        );

        return _vault;
    }
```

Recommendation: Implement a counter and append the count to the `name` & `symbol` fields for the deployed vaults via the `PrizeVaultFactory` contract.