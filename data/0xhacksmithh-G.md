### [Gas-01] No need to check `_shares == 0` & `_assets == 0` inside `_depositAndMint()` and `_burnAndWithdraw()`

As we hcan see
_shares = previewDeposit(_assets);
and
_assets = previewMint(_shares);
where they are `1:1` as per code comment

so basically if _shares != 0 so that also same for _assets 

So check for only one of them will sufficient.
```file: PrizeVault.sol```
```diff
function _depositAndMint(address _caller, address _receiver, uint256 _assets, uint256 _shares) internal {
        if (_shares == 0) revert MintZeroShares();
        if (_assets == 0) revert DepositZeroAssets();
```
```diff
function _burnAndWithdraw(
        address _caller,
        address _receiver,
        address _owner,
        uint256 _shares,
        uint256 _assets
    ) internal {
        if (_assets == 0) revert WithdrawZeroAssets(); // @audit gas
        if (_shares == 0) revert BurnZeroShares();
        if (_caller != _owner) {
            _spendAllowance(_owner, _caller, _shares);
```
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L843-L845

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L894-L895
```

### [Gas-02] some state variable could be `immutable`
As variable only set once.
```file: Claimable.sol```
```diff
-   address public claimer;

+   address public immutable claimer;
```
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L27
```

### [Gas-03] Instaed of reading state variable multiple times, that state variable could be cached in memory
#### `claimer` an cached in memory

```file: Claimable.sol```
```diff
   modifier onlyClaimer() {
+       address _claimer = claimer;
-       if (msg.sender != claimer) revert CallerNotClaimer(msg.sender, claimer);

+       if (msg.sender != _claimer) revert CallerNotClaimer(msg.sender, _claimer); 
        _;
    }
```
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L53
```

#### `liquidationPair` and `yieldFeeRecipient` could be cached in memory
```file: PrizeVault.sol```
```diff
    modifier onlyLiquidationPair() {
        if (msg.sender != liquidationPair) { // @audit dual state read
            revert CallerNotLP(msg.sender, liquidationPair);
        }
        _;
    }

    /// @notice Requires the caller to be the yield fee recipient.
    modifier onlyYieldFeeRecipient() {
        if (msg.sender != yieldFeeRecipient) { // @audit dual state read
            revert CallerNotYieldFeeRecipient(msg.sender, yieldFeeRecipient);
        }
        _;
    }
```
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L260-L273
```

### [Gas-04] some mapping results could be cached in memory rather than recalling those again and again

#### `_hooks[_winner]` could be stored in memory
```file: Claimable.sol```
```diff
if (_hooks[_winner].useBeforeClaimPrize) { // @audit _hooks[_winner]
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

        if (recipient == address(0)) revert ClaimRecipientZeroAddress();

        uint256 prizeTotal = prizePool.claimPrize(
            _winner,
            _tier,
            _prizeIndex,
            recipient,
            _reward,
            _rewardRecipient
        );

        if (_hooks[_winner].useAfterClaimPrize) {
            _hooks[_winner].implementation.afterClaimPrize{ gas: HOOK_GAS }(
                _winner,
                _tier,
                _prizeIndex,
                prizeTotal,
                recipient
            );
```
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L85-L115
```

### [Gas-05] internal function only once used could be inlined
`_setClaimer()` is a inernal function which only called once in constructor, and never sed further could be inlined
```file: Claimable.sol```
```diff
    function _setClaimer(address _claimer) internal { // @audit inlined
        if (_claimer == address(0)) revert ClaimerZeroAddress();
        claimer = _claimer;
        emit ClaimerSet(_claimer);
    }
```
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L128-L132
```

### [Gas-06] Arithmatic operation could be unchecked
```file: PrizeVaultFactory.sol```
```diff
        PrizeVault _vault = new PrizeVault{
            salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++)) 
        }(
```
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L103
```

### [Gas-07] Instaed of using state variable in `revert`, first local cache memory varible could used
```file: PrizeVault.sol```
```diff
    modifier onlyLiquidationPair() {
        if (msg.sender != liquidationPair) { // @audit dual state read
            revert CallerNotLP(msg.sender, liquidationPair);
        }
        _;
    }

    /// @notice Requires the caller to be the yield fee recipient.
    modifier onlyYieldFeeRecipient() {
        if (msg.sender != yieldFeeRecipient) { // @audit dual state read
            revert CallerNotYieldFeeRecipient(msg.sender, yieldFeeRecipient);
        }
        _;
    }
```
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L260-L273
```

### [Gas-08]
```file: PrizeVault.sol```
#### `yieldVault` could be cached in memory
```diff
function totalAssets() public view returns (uint256) { // @audit yieldVault
        return yieldVault.convertToAssets(yieldVault.balanceOf(address(this))) + _asset.balanceOf(address(this));
    }
```
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L336-L338
```

#### `prizePool` could cached in memory rather than recalling it again and again
```diff
) external onlyLiquidationPair {
        address _prizeToken = address(prizePool.prizeToken()); // @audit prizePool
        if (_tokenIn != _prizeToken) {
            revert LiquidationTokenInNotPrizeToken(_tokenIn, _prizeToken);
        }

        prizePool.contributePrizeTokens(address(this), _amountIn);
    }
```
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L708
```

