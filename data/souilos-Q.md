# Claimers could not get the fees because of a malicious hook

## Impact
Claimers could pay the gas for transaction when calling ```Claimable.sol::claimPrize``` without being rewarded (by the fees).

## Proof of Concept
Once a winner in a draw has been set, then anyone (bots) can call ```Claimable.sol::claimPrize``` to send the reward to the winner.

```solidity
    /// @dev Also calls the before and after claim hooks if set by the winner.
    function claimPrize(
        address _winner,
        uint8 _tier,
        uint32 _prizeIndex,
        uint96 _reward,
        address _rewardRecipient
    ) external onlyClaimer returns (uint256) {
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
        }

        return prizeTotal;
    }
```

However has seen in the code any participant in the draw can create **hooks** to make actions BEFORE or AFTER ```Claimable.sol::claimPrize```
is called.

https://github.com/GenerationSoftware/pt-v5-vault/blob/94b0c034c68b5318a25211a7b9f6d9ff6693e6ab/src/abstract/HookManager.sol#L29 by using ```HookManager.sol::setHooks```:

```solidity
    /// @notice Sets the hooks for a winner.
    /// @dev Emits a `SetHooks` event
    /// @param hooks The hooks to set
    function setHooks(VaultHooks calldata hooks) external {
        _hooks[msg.sender] = hooks;
        emit SetHooks(msg.sender, hooks);
    }
```

A malicious hook created by the winner could call ```Claimable.sol::claimPrize``` before it's executed by the initial claimer.
This would result in the initial claimer paying the gas for the transaction, but the fees would be sent to the winner.

## Tools Used
Manual review.

## Recommended Mitigation Steps
Use a protection in some functions to make sure the hook has limited capabilities. The hook could be used by winners but must not be able to call certains functions such as ```Claimable.sol::claimPrize```.