

## 1 `PrizeVault->Claimable->HookManager::setHooks() ` allows Unintended or Malicious Use of Prize Winners' Hook
The setHooks function in Vault.sol allows users to set arbitrary hooks, potentially enabling them to make external calls with unintended consequences. This vulnerability could lead to various unexpected behaviors, such as unauthorized side transactions with gas paid unbeknownst to the claimer, reentrant calls, or denial-of-service attacks on claiming transactions.

```solidity
 function setHooks(VaultHooks calldata hooks) external {
        _hooks[msg.sender] = hooks;
        emit SetHooks(msg.sender, hooks);
    }
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/abstract/HookManager.sol#L29
it is possible for a hook to make an external call and modify the EVM state.

Handle these malicious calls

## 2 `PrizeVaultFactory::deployVault` allows deployment of vaults with non-authentic `claimer`, `prizePool` etc


A malicious vault can be deployed via the official PrizeVaultFactory contract. Users may lose funds while interacting with such vaults.

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L92

ensure proper validation or implement kind of trusted role for the deployement of new vaults 


## 3 `PrizeVault::setYieldFeeRecipient` doesn't check for zero address 

```solidity
    function setYieldFeeRecipient(address _yieldFeeRecipient) external onlyOwner {
        _setYieldFeeRecipient(_yieldFeeRecipient);
    }
    function _setYieldFeeRecipient(address _yieldFeeRecipient) internal {
        //@audit lacks zero address check
        yieldFeeRecipient = _yieldFeeRecipient;
        emit YieldFeeRecipientSet(_yieldFeeRecipient);
    }

```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L958

## 4 `depositWithPermit` doesn't server the purpose of permit function since even after sign the tx only signer can call this function
```solidity
    function depositWithPermit(
        uint256 _assets,
        address _owner,
        uint256 _deadline,
        uint8 _v,
        bytes32 _r,
        bytes32 _s
    ) external returns (uint256) {
        if (_owner != msg.sender) {
            revert PermitCallerNotOwner(msg.sender, _owner);
        }

        // Skip the permit call if the allowance has already been set to exactly what is needed. This prevents
        // griefing attacks where the signature is used by another actor to complete the permit before this
        // function is executed.
        if (_asset.allowance(_owner, address(this)) != _assets) {
            IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);/
        }

        uint256 _shares = previewDeposit(_assets);
        _depositAndMint(_owner, _owner, _assets, _shares);
        return _shares;
    }
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L532
fix this so that it serves the purpose of permit function or remove the function to save deployment cost

## 5 `depositWithPermit` lacks chainID which  allows signatures to be re-used across forks


```solidity
    function depositWithPermit(
        uint256 _assets,
        address _owner,
        uint256 _deadline,
        uint8 _v,
        bytes32 _r,
        bytes32 _s
    ) external returns (uint256) {
///////////////////////
        if (_asset.allowance(_owner, address(this)) != _assets) {
            IERC20Permit(address(_asset)).permit(_owner, address(this), _assets, _deadline, _v, _r, _s);/
        }
///////////////////////
    }
```
https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L540
eg. 

Scenario :: An EIP is included in an upcoming hard fork that has split
the community. After the hard fork, a significant user base remains on the old chain. On
the new chain, Alice approves some tokens via a call to permit. Alice,
operating on both chains, replays the permit call on the old chain 


Include the chainID opcode in the permit schema. This will make replay
attacks impossible in the event of a post-deployment hard fork.


