# Gas Optimizations

**Note : _G-05_ contains only those instances which were missed by bot. Since they are major gas savings so I included those missed instances**

## Table of Contents

- [G-01] [Refactor `depositWithPermit` function to remove `if` statement(**saves ~40 Gas**)](#g-01-refactor-depositwithpermit-function-to-remove-if-statementsaves-40-gas)

- [G-02] [Make `abi.encodeWithSelector(IERC20Metadata.decimals.selector)` constant instead of calculating it every time `_tryGetAssetDecimals` function will be called.(**Gas Saved ~75 Gas**)](#g-02-make-abiencodewithselectorierc20metadatadecimalsselector-constant-instead-of-calculating-it-every-time-_trygetassetdecimals-function-will-be-calledgas-saved-75-gas)

- [G-03] [Refactor `claimYieldFeeShares` function to save `1 checked subtraction` and `1 SLOAD`(**Gas Saved ~75 Gas**)](#g-03-refactor-claimyieldfeeshares-function-to-save-1-checked-subtraction-and-1-sloadgas-saved-250-gas)

- [G-04] [Call directly internal `_totalDebt` function instead of calling public `totalDebt` function to `avoid function call` in call stack.](#g-04-call-directly-internal-_totaldebt-function-instead-of-calling-public-totaldebt-function-to-avoid-function-call-in-call-stack)

- **[G-05]** [State variables should be cached in stack variables rather than re-reading them from storage( _Instances Missed by Bot_ ) (**Gas Saved ~300 Gas**)](#g-05-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storageinstances-missed-by-bot-gas-saved-300-gas)
- [G-06] [Remove unnecessary stack variables in constructor](#g-06-remove-unnecessary-stack-variables-in-constructor)
- [G-07] [Refactor `PrizeVault` contract `constructor` to fail early saves (~3k Gas) if check fails.](#g-07-refactor-prizevault-contract-constructor-to-fail-early-saves-3k-gas-if-check-fails)
- [G-08] [Avoid calling functions in same contract which just returns same function parameter without changing or doing anything(saves ~180 Gas)](#g-08-avoid-calling-functions-in-same-contract-which-just-returns-same-function-parameter-without-changing-or-doing-anythingsaves-180-gas)
- [G-09] [No need to cache global variable `msg.sender`, accessing it directly is cheaper](#g-09-no-need-to-cache-global-variable-msgsender-accessing-it-directly-is-cheaper)
- [G-10] [Do not cache `immutable` variables](#g-10-do-not-cache-immutable-variables)
- [G-11] [Cache the `calculation` instead of `re-calculating` same variables (Saves ~180 Gas)](#g-11-cache-the-calculation-instead-of-re-calculating-same-variables-saves-180-gas)

- [G-12] [Refactor `transferTokensOut` function to fail early by implementing if checks in start.(saves ~10k Gas avg.) Half of the times](#g-12-refactor-transfertokensout-function-to-fail-early-by-implementing-if-checks-in-startsaves-10k-gas-avg-half-of-the-times)

## Auditor's Disclaimer : 

All these findings are good findings and 100% safe to implement at no security/logic risk. They all are found by thorough manual review. Except G-05 all are unique from bot. Even G-05  only covers those instances which were missed by bot.

## [G-01] Refactor `depositWithPermit` function to remove `if` statement(**saves ~40 Gas**)

Remove `if (_owner != msg.sender)` check and use `msg.sender` instead of `_owner`. Since it is required that passed `_owner` param should be `msg.sender`. So we can achieve that by just using `msg.sender` where ever `_owner` param used in this `depositWithPermit` function. It will save 1 `if` check. And accessing msg.sender is 1 gas cheaper than accessing from local variable.(**saves ~40 Gas**)

```solidity
File : src/PrizeVault.sol

524: function depositWithPermit(
525:     uint256 _assets,
526:     address _owner,
527:     uint256 _deadline,
528:     uint8 _v,
529:     bytes32 _r,
530:     bytes32 _s
531: ) external returns (uint256) {
532:     if (_owner != msg.sender) {
533:         revert PermitCallerNotOwner(msg.sender, _owner);
534:     }
535:
536:        // Skip the permit call if the allowance has already been set to exactly what is needed. This prevents
537:        // griefing attacks where the signature is used by another actor to complete the permit before this
538:        // function is executed.
539:     if (_asset.allowance(_owner, address(this)) != _assets) {
540:        IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);
541:      }
542:
543:     uint256 _shares = previewDeposit(_assets);
544:    _depositAndMint(_owner, _owner, _assets, _shares);
545:     return _shares;
546:  }

```

[PrizeVault.sol#L524-L546](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L524C5-L546C6)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

524: function depositWithPermit(
525:     uint256 _assets,
-526:     address _owner,
+526:     address /*_owner*/,
527:     uint256 _deadline,
528:     uint8 _v,
529:     bytes32 _r,
530:     bytes32 _s
531: ) external returns (uint256) {
-532:     if (_owner != msg.sender) {
-533:         revert PermitCallerNotOwner(msg.sender, _owner);
-534:     }
535:
536:        // Skip the permit call if the allowance has already been set to exactly what is needed. This prevents
537:        // griefing attacks where the signature is used by another actor to complete the permit before this
538:        // function is executed.
-539:     if (_asset.allowance(_owner, address(this)) != _assets) {
-540:        IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);
+539:     if (_asset.allowance(msg.sender, address(this)) != _assets) {
+540:        IERC20Permit(address(_asset)).permit(msg.sender, address(this), _assets, _deadline, _v, _r, _s);
541:      }
542:
543:     uint256 _shares = previewDeposit(_assets);
-544:    _depositAndMint(_owner, _owner, _assets, _shares);
+544:    _depositAndMint(msg.sender, _owner, _assets, _shares);
545:     return _shares;
546:  }

```

## [G-02] Make `abi.encodeWithSelector(IERC20Metadata.decimals.selector)` constant instead of calculating it every time `_tryGetAssetDecimals` function will be called.(**Gas Saved ~75 Gas**)

Since interface IERC20Metadata is fixed and imported in `PrizeVault.sol` also `decimals` function in it. So decimals selector will be fixed. So every time _tryGetAssetDecimals function will be called `abi.encodeWithSelector(IERC20Metadata.decimals.selector)` will give same output by calculating it. So it is better to store this fix output of this encoding into constant. It can save this calculating encoding gas(almost 75 gas) every time this `_tryGetAssetDecimals` function will be called. (**Gas Saved ~75 Gas**).

```solidity
File : src/PrizeVault.sol

772:  function _tryGetAssetDecimals(IERC20 asset_) internal view returns (bool, uint8) {
773:    (bool success, bytes memory encodedDecimals) = address(asset_).staticcall(
774:       abi.encodeWithSelector(IERC20Metadata.decimals.selector)
775:    );

```

[PrizeVault.sol#L772-L775](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L772C1-L775C11)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

+    bytes constant encodeWithSelector = abi.encodeWithSelector(IERC20Metadata.decimals.selector);
772:  function _tryGetAssetDecimals(IERC20 asset_) internal view returns (bool, uint8) {
773:    (bool success, bytes memory encodedDecimals) = address(asset_).staticcall(
-774:       abi.encodeWithSelector(IERC20Metadata.decimals.selector)
+774:       encodeWithSelector
775:    );

```

## [G-03] Refactor `claimYieldFeeShares` function to save `1 checked subtraction` and `1 SLOAD`(**Gas Saved ~250 Gas**)

Instead of subtracting the same value from storage after caching it directly assign `0` to this storage var.
`yieldFeeBalance` is cached into `_yieldFeeBalance` local variable. And when this `_yieldFeeBalance` will be subtracted from state var. `yieldFeeBalance` at line 617 below it will become `0` since no value changed after that caching at line 614. So it is efficient to assign 0 directly into `yieldFeeBalance`. So we can save 1 SLOAD
and 1 checked subtraction. 

#### Total Gas Saved  ~250 Gas (100 for 1 SLOAD and ~150 for checked subtraction)

```solidity
File : src/PrizeVault.sol

611: function claimYieldFeeShares(uint256 _shares) external onlyYieldFeeRecipient {
612:   if (_shares == 0) revert MintZeroShares();
613:
614:    uint256 _yieldFeeBalance = yieldFeeBalance;
615:    if (_shares > _yieldFeeBalance) revert SharesExceedsYieldFeeBalance(_shares, _yieldFeeBalance);
616:
617:    yieldFeeBalance -= _yieldFeeBalance;

```

[PrizeVault.sol#L611-L618](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L611C5-L618C1)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

611: function claimYieldFeeShares(uint256 _shares) external onlyYieldFeeRecipient {
612:   if (_shares == 0) revert MintZeroShares();
613:
614:    uint256 _yieldFeeBalance = yieldFeeBalance;
615:    if (_shares > _yieldFeeBalance) revert SharesExceedsYieldFeeBalance(_shares, _yieldFeeBalance);
616:
-617:    yieldFeeBalance -= _yieldFeeBalance;
+617:    yieldFeeBalance = 0;

```

## [G-04] Call directly internal `_totalDebt` function instead of calling public `totalDebt` function to `avoid function call` in call stack.

Since `totalDebt` is calling `_totalDebt(totalSupply())` and both are present in same contract.Where both can be called in that contract internal/public so it is convenient to call directly internal function in that contract instead of calling public which in the end only calling that internal function. Saves  gas.

```solidity
File : src/PrizeVault.sol

function totalDebt() public view returns (uint256) {
        return _totalDebt(totalSupply());
    }

```

We can directly call `_totalDebt(totalSupply())` instead of calling `totalDebt` public function.

[PrizeVault.sol#L573-L575](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L573C5-L575C6)

### Instance#1 Avoid `1 function call` in `convertToShares` function

```solidity
File : src/PrizeVault.sol

341:  function convertToShares(uint256 _assets) public view returns (uint256) {
342:    uint256 totalDebt_ = totalDebt();
343:    uint256 _totalAssets = totalAssets();

```

[PrizeVault.sol#L341-L343](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L341C1-L343C46)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

341:  function convertToShares(uint256 _assets) public view returns (uint256) {
-342:    uint256 totalDebt_ = totalDebt();
+342:    uint256 totalDebt_ = _totalDebt(totalSupply());
343:    uint256 _totalAssets = totalAssets();

```

### Instance#2 Avoid `1 function call` in `convertToAssets` function

```solidity
File : src/PrizeVault.sol

355:  function convertToAssets(uint256 _shares) public view returns (uint256) {
356:    uint256 totalDebt_ = totalDebt();

```

[PrizeVault.sol#L355-L356](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L355-L356)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

355:  function convertToAssets(uint256 _shares) public view returns (uint256) {
-356:    uint256 totalDebt_ = totalDebt();
+356:    uint256 totalDebt_ = _totalDebt(totalSupply());

```

## [G-05] State variables should be cached in stack variables rather than re-reading them from storage(*Instances Missed by Bot*) (**Gas Saved ~300 Gas**)

### `SAVE: 300 GAS, 3 SLOAD`

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable.

### `claimer` can be cached to save 1 SLOAD ~100 Gas ( 97 Gas technically)

```solidity
File : src/abstract/Claimable.sol

52:  modifier onlyClaimer() {
53:      if (msg.sender != claimer) revert CallerNotClaimer(msg.sender, claimer);
54:      _;
55:  }
```

[Claimable.sol#L52-L55](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L52C1-L55C6)

**Recommended Mitigation Steps:**

```diff
File : src/abstract/Claimable.sol

52:  modifier onlyClaimer() {
+       address _claimer = claimer;

-53:      if (msg.sender != claimer) revert CallerNotClaimer(msg.sender, claimer);
+53:      if (msg.sender != _claimer) revert CallerNotClaimer(msg.sender, _claimer);
54:      _;
55:  }
```

### `liquidationPair` can be cached to save 1 SLOAD ~100 Gas ( 97 Gas technically)

```solidity
File : src/PrizeVault.sol

260: modifier onlyLiquidationPair() {
261:      if (msg.sender != liquidationPair) {
262:          revert CallerNotLP(msg.sender, liquidationPair);
263:      }
264:      _;
265:  }

```

[PrizeVault.sol#L260-L265](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L260C3-L265C6)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

260: modifier onlyLiquidationPair() {
+     address _liquidationPair = liquidationPair;

-261:      if (msg.sender != liquidationPair) {
-262:          revert CallerNotLP(msg.sender, liquidationPair);
+261:      if (msg.sender != _liquidationPair) {
+262:          revert CallerNotLP(msg.sender, _liquidationPair);
263:      }
264:      _;
265:  }
```

### `yieldFeeRecipient` can be cached to save 1 SLOAD ~100 Gas ( 97 Gas technically)

```solidity
File : src/PrizeVault.sol

268:  modifier onlyYieldFeeRecipient() {
269:      if (msg.sender != yieldFeeRecipient) {
270:          revert CallerNotYieldFeeRecipient(msg.sender, yieldFeeRecipient);
271:      }
272:      _;
273:   }
```

[PrizeVault.sol#L268-L273](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L268C1-L273C6)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

268:  modifier onlyYieldFeeRecipient() {
+      address _yieldFeeRecipient = yieldFeeRecipient;

-269:      if (msg.sender != yieldFeeRecipient) {
-270:          revert CallerNotYieldFeeRecipient(msg.sender, yieldFeeRecipient);
+269:      if (msg.sender != _yieldFeeRecipient) {
+270:          revert CallerNotYieldFeeRecipient(msg.sender, _yieldFeeRecipient);
271:      }
272:      _;
273:   }
```

## [G-06] Remove unnecessary stack variables in constructor

### Remove `asset_` stack variable and use `_asset` immutable variable direct instead of `asset_`

```solidity
File : src/PrizeVault.sol

303: IERC20 asset_ = IERC20(yieldVault_.asset());
304: (bool success, uint8 assetDecimals) = _tryGetAssetDecimals(asset_);
305: _underlyingDecimals = success ? assetDecimals : 18;
306: _asset = asset_;

```

[PrizeVault.sol#L303-L306](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L303C8-L306C25)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

-303: IERC20 asset_ = IERC20(yieldVault_.asset());
+      _asset = IERC20(yieldVault_.asset());

-304: (bool success, uint8 assetDecimals) = _tryGetAssetDecimals(asset_);
+304: (bool success, uint8 assetDecimals) = _tryGetAssetDecimals(_asset);
305: _underlyingDecimals = success ? assetDecimals : 18;
-306: _asset = asset_;

```

## [G-07] Refactor `PrizeVault` contract `constructor` to fail early saves (~3k Gas) if check fails.

### Place `_setYieldFeePercentage` function call above to the `IERC20(yieldVault_.asset())` to avoid external calls to fail early if failing.

Since the `_setYieldFeePercentage` function Perform a constant check to ensure that the input `_yieldFeePercentage` is within the allowed range (not exceeding `MAX_YIELD_FEE`). It is recommened to write this  `_setYieldFeePercentage` in constructor before any external call. To check early if failing or not.

```solidity
File : src/PrizeVault.sol

289:    constructor(
...
     {
300:    if (address(yieldVault_) == address(0)) revert YieldVaultZeroAddress();
301:    if (owner_ == address(0)) revert OwnerZeroAddress();
302:
303:     IERC20 asset_ = IERC20(yieldVault_.asset());
...
310:
311:    _setYieldFeeRecipient(yieldFeeRecipient_);
312:    _setYieldFeePercentage(yieldFeePercentage_);
313:    }

```

[PrizeVault.sol#L289-L313](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L289C1-L313C6)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

289:    constructor(
...
     {
300:    if (address(yieldVault_) == address(0)) revert YieldVaultZeroAddress();
301:    if (owner_ == address(0)) revert OwnerZeroAddress();
302:
+312:    _setYieldFeePercentage(yieldFeePercentage_);
303:     IERC20 asset_ = IERC20(yieldVault_.asset());
...
310:
311:    _setYieldFeeRecipient(yieldFeeRecipient_);
-312:    _setYieldFeePercentage(yieldFeePercentage_);
313:    }

```

## [G-08] Avoid calling functions in same contract which just returns same function parameter without changing or doing anything(saves ~180 Gas)

Since these function are for to make PrizeVault EIP4626 compatible but within prizeVault if call to these  functions which are not doing anything just return same passed param back can be avoided when calling in same PrizeVault since ewe know in our they will return the same value so use that same value directly.

**Saves ~30 Gas Per Instance Total ~180 Gas**

### Instance#1 No need to call `previewDeposit`

```solidity
File : src/PrizeVault.sol

441:  function previewDeposit(uint256 _assets) public pure returns (uint256) {
442:    // shares represent how many assets an account has deposited, so they are 1:1 on deposit
443:     return _assets;
444:    }
```

[PrizeVault.sol#L441-L444](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L441C1-L444C6)

Since it is returning the same function parameter without any change.

#### Instance#1.1

```solidity
File : src/PrizeVault.sol

475:  function deposit(uint256 _assets, address _receiver) external returns (uint256) {
476:     uint256 _shares = previewDeposit(_assets);
477:    _depositAndMint(msg.sender, _receiver, _assets, _shares);
478:      return _shares;
479:    }

```

[PrizeVault.sol#L475-L479](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L475C1-L479C6)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

475:  function deposit(uint256 _assets, address _receiver) external returns (uint256) {
-476:     uint256 _shares = previewDeposit(_assets);
+476:     uint256 _shares = _assets;
477:    _depositAndMint(msg.sender, _receiver, _assets, _shares);
478:      return _shares;
479:    }

```

#### Instance#1.2

```solidity
File : src/PrizeVault.sol

524: function depositWithPermit(
...
543:     uint256 _shares = previewDeposit(_assets);
544:    _depositAndMint(_owner, _owner, _assets, _shares);
545:     return _shares;
546:  }

```

[PrizeVault.sol#L543-L546](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L543-L546)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

524: function depositWithPermit(
...
-543:     uint256 _shares = previewDeposit(_assets);
+543:     uint256 _shares = _assets;
544:    _depositAndMint(_owner, _owner, _assets, _shares);
545:     return _shares;
546:  }

```

#### Instance#1.3

```solidity
File : src/PrizeVault.sol

552:   function sponsor(uint256 _assets) external returns (uint256) {
553:    address _owner = msg.sender;
554:
555:    uint256 _shares = previewDeposit(_assets);
```

[PrizeVault.sol#L552-L555](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L552-L555)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

552:   function sponsor(uint256 _assets) external returns (uint256) {
553:    address _owner = msg.sender;
554:
-555:    uint256 _shares = previewDeposit(_assets);
+555:    uint256 _shares = _assets;
```

### Instance#2 No need to call `previewMint`

```solidity
File : src/PrizeVault.sol

447:  function previewMint(uint256 _shares) public pure returns (uint256) {
448:    // shares represent how many assets an account has deposited, so they are 1:1 on mint
449:    return _shares;
450:    }
```

[PrizeVault.sol#L447-L450](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L447C1-L450C6)

Since it is returning the same function parameter without any change.

#### Instance#2.1

```solidity
File : src/PrizeVault.sol

482: function mint(uint256 _shares, address _receiver) external returns (uint256) {
483:   uint256 _assets = previewMint(_shares);

```

[PrizeVault.sol#L482-L483](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L482C5-L483C48)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

482: function mint(uint256 _shares, address _receiver) external returns (uint256) {
-483:   uint256 _assets = previewMint(_shares);
+483:   uint256 _assets = _shares;

```

## [G-09] No need to cache global variable `msg.sender`, accessing it directly is cheaper

Using msg.sender directly Saves 1 unit Gas per access and also avoid stack var. creation.

```solidity
File : src/PrizeVault.sol

552:   function sponsor(uint256 _assets) external returns (uint256) {
553:    address _owner = msg.sender;
554:
555:    uint256 _shares = previewDeposit(_assets);
556:    _depositAndMint(_owner, _owner, _assets, _shares);
557:
558:    if (twabController.delegateOf(address(this), _owner) != SPONSORSHIP_ADDRESS) {
559:         twabController.sponsor(_owner);
560:     }
561:
562:    emit Sponsor(_owner, _assets, _shares);
563:
564:    return _shares;
565:  }

```

[PrizeVault.sol#L552-L565](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L552C1-L565C6)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

552:   function sponsor(uint256 _assets) external returns (uint256) {
-553:    address _owner = msg.sender;
554:
555:    uint256 _shares = previewDeposit(_assets);
-556:    _depositAndMint(_owner, _owner, _assets, _shares);
+556:    _depositAndMint(msg.sender, msg.sender, _assets, _shares);
557:
-558:    if (twabController.delegateOf(address(this), _owner) != SPONSORSHIP_ADDRESS) {
-559:         twabController.sponsor(_owner);
+558:    if (twabController.delegateOf(address(this), msg.sender) != SPONSORSHIP_ADDRESS) {
+559:         twabController.sponsor(msg.sender);
560:     }
561:
-562:    emit Sponsor(_owner, _assets, _shares);
+562:    emit Sponsor(msg.sender, _assets, _shares);
563:
564:    return _shares;
565:  }

```

## [G-10] Do not cache `immutable` variables

Caching immutable variables can lead to increased gas consumption and storage costs, as it creates redundant copies of values that are already known at compile-time. Instead, developers should access immutable variables directly when needed, avoiding unnecessary storage and computation. By adhering to this best practice, gas efficiency is improved, resulting in more cost-effective and optimized smart contracts on the Ethereum blockchain.

### Instance#1

```solidity
File : src/PrizeVault.sol

599:  uint256 _yieldBuffer = yieldBuffer;
600:   if (totalYieldBalance_ >= _yieldBuffer) {
601:      return _yieldBuffer;

```

[PrizeVault.sol#L599-L601](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L599C1-L601C33)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

-599:  uint256 _yieldBuffer = yieldBuffer;
-600:   if (totalYieldBalance_ >= _yieldBuffer) {
-601:      return _yieldBuffer;

+600:   if (totalYieldBalance_ >= yieldBuffer) {
+601:      return yieldBuffer;

```

### Instance#2

```solidity
File : src/PrizeVault.sol

823: function _availableYieldBalance(
...
825:    uint256 _yieldBuffer = yieldBuffer;
826:     if (totalYieldBalance_ >= _yieldBuffer) {
827:        unchecked {
828:           return totalYieldBalance_ - _yieldBuffer;
829:       }

```

[PrizeVault.sol#L823-L829](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L823C1-L829C14)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

823: function _availableYieldBalance(
...
-825:    uint256 _yieldBuffer = yieldBuffer;
-826:     if (totalYieldBalance_ >= _yieldBuffer) {
+826:     if (totalYieldBalance_ >= yieldBuffer) {
827:        unchecked {
-828:           return totalYieldBalance_ - _yieldBuffer;
+828:           return totalYieldBalance_ - yieldBuffer;
829:       }

```

## [G-11] Cache the `calculation` instead of `re-calculating` same variables (Saves ~180 Gas)

Caching the calculation rather than repeatedly recalculating the same variables involves storing the result of a computation in a temporary variable or cache. 

Cache `_amountOut + _yieldFee` instead of re-calculating it. Avoids 1 checked addition operation (**Gas Saved ~180 Gas**)

```solidity
File : src/PrizeVault.sol

679: if (_amountOut + _yieldFee > _availableYield) {
680:   revert LiquidationExceedsAvailable(_amountOut + _yieldFee, _availableYield);
681:   }

```

[PrizeVault.sol#L679-L681](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L679C1-L681C10)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

+    uint256 totalAmountOut_YieldFee = _amountOut + _yieldFee;
-679: if (_amountOut + _yieldFee > _availableYield) {
-680:   revert LiquidationExceedsAvailable(_amountOut + _yieldFee, _availableYield);
+679: if (totalAmountOut_YieldFee > _availableYield) {
+680:   revert LiquidationExceedsAvailable(totalAmountOut_YieldFee, _availableYield);
681:   }

```

## [G-12] Refactor `transferTokensOut` function to fail early by implementing if checks in start.(saves ~10k Gas avg.) Half of the times.

Refactor the code to fail early if the check can be implemented without any additional operations. If `_tokenOut` is neither the address of `_asset` nor the address of this contract then this will revert with `LiquidationTokenOutNotSupported` error. So it is better to revert as early as possible instead of reverting after wasting many Gas in function call, SSTORE and multiple other operations in below code. Save ~10k gas avg. if failing with this reason.

```solidity
File : src/PrizeVault.sol
659: function transferTokensOut( 
       ....

       uint256 _availableYield = availableYieldBalance();
        uint32 _yieldFeePercentage = yieldFeePercentage;

        // Determine the proportional yield fee based on the amount being liquidated:
        uint256 _yieldFee;
        if (_yieldFeePercentage != 0) {
            // The yield fee is calculated as a portion of the total yield being consumed, such that 
            // `total = amountOut + yieldFee` and `yieldFee / total = yieldFeePercentage`. 
            _yieldFee = (_amountOut * FEE_PRECISION) / (FEE_PRECISION - _yieldFeePercentage) - _amountOut;
        }

        // Ensure total liquidation amount does not exceed the available yield balance:
        if (_amountOut + _yieldFee > _availableYield) {
            revert LiquidationExceedsAvailable(_amountOut + _yieldFee, _availableYield);
        }

        // Increase yield fee balance:
        if (_yieldFee > 0) {
            yieldFeeBalance += _yieldFee;
        }
689:  if (_tokenOut == address(_asset)) {
690:     _withdraw(_receiver, _amountOut);
691:   } else if (_tokenOut == address(this)) {
692:       _mint(_receiver, _amountOut);
693:    } else {
694:       revert LiquidationTokenOutNotSupported(_tokenOut);
695:    }

```

[PrizeVault.sol#L659-L695](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L659-L695)

**Recommended Mitigation Steps:**

```diff
File : src/PrizeVault.sol

+    if (_tokenOut != address(_asset) && _tokenOut != address(this)) {
+        revert LiquidationTokenOutNotSupported(_tokenOut);
+    }

        uint256 _availableYield = availableYieldBalance();
        uint32 _yieldFeePercentage = yieldFeePercentage;

        // Determine the proportional yield fee based on the amount being liquidated:
        uint256 _yieldFee;
        if (_yieldFeePercentage != 0) {
            // The yield fee is calculated as a portion of the total yield being consumed, such that 
            // `total = amountOut + yieldFee` and `yieldFee / total = yieldFeePercentage`. 
            _yieldFee = (_amountOut * FEE_PRECISION) / (FEE_PRECISION - _yieldFeePercentage) - _amountOut;
        }

        // Ensure total liquidation amount does not exceed the available yield balance:
        if (_amountOut + _yieldFee > _availableYield) {
            revert LiquidationExceedsAvailable(_amountOut + _yieldFee, _availableYield);
        }

        // Increase yield fee balance:
        if (_yieldFee > 0) {
            yieldFeeBalance += _yieldFee;
        }

689:  if (_tokenOut == address(_asset)) {
690:     _withdraw(_receiver, _amountOut);
-691:   } else if (_tokenOut == address(this)) {
+691:   } else {
692:       _mint(_receiver, _amountOut);
-693:    } else {
-694:       revert LiquidationTokenOutNotSupported(_tokenOut);
-695:    }

```
