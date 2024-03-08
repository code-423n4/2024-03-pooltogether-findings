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