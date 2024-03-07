# PoolTogether QA report by Timenov

## Summary

| |Issue|Instances|
|-|:-|:-:|
| I-01 | Incorrect allowance check. | 1 |
| I-02 | Return value is empty. | 1 |
| I-03 | Substraction of smaller uint. | 1 |
| L-01 | Big gas limit. | 1 |

### [I-01] Incorrect allowance check.
In `PrizeVault::depositWithPermit` the user has the ability to deposit asset with the `permit` function. The code checks if `_asset.allowance(_owner, address(this)) != _assets` and if this is `false`, `permit` is called. However if the `allowance` is bigger than `_assets`, `permit` will be called. This should not happen, so consider using `<` instead of `!=` for the allowance check.

### [I-02] Return value is empty.
If `PrizeVault::transferTokensOut` is successful, the return value will be `""`. This is not good as it will not give clear output if the function is executed successfully. Consider returning meaningful message.

### [I-03] Substraction of smaller uint.
In `PrizeVault::_twabSupplyLimit` will return `type(uint96).max - _totalSupply`. The problem is that, this operation is in `unchecked` block and the `_totalSupply` is of `uint256`. This can underflow silently and return wrong values.

### [L-01] Big gas limit.
In `Claimable.sol` there is a constant variable `HOOK_GAS`, which is used for the max gas used upon calling `beforeClaimPrize` and `afterClaimPrize`. The value is `150_000` which is big for L2 chains like Optimism, Arbitrum and Base. Malicious prize winner can use all of the gas for the expense of the `claimer`. Consider adding additional parameter to the function `claimPrize` to allow the `claimer` to choose the gas he wants to spend on both of the calls.

###### PoC on how this can happen:
1. Create [WinnerMock.sol](https://gist.github.com/Pavel2202/045730e40f68785dbef9891505c891e9) in the test folder.
2. Comment a few lines in [claimPrize](https://gist.github.com/Pavel2202/637a5f010c3c74651f15931d54997ef1)
3. Add the following [test case](https://gist.github.com/Pavel2202/f502b91b0f6d8fce10c9d21eaa7cb155) to `Claimable.t.sol`.
4. Run `forge test --match-test testClaimPrize_drainGas -vvvv`
5. Gas spent is `355298`