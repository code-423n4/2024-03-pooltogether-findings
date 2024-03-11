# Low-Severity

## [L-01] Solidity version 0.8.24 will not work on other evm chains due to push0 opcode use 0.8.19

```solidity
File : src/interfaces/IVaultHooks.sol

02:    pragma solidity ^0.8.24;

```

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/interfaces/IVaultHooks.sol#L2C1-L2C25

## [L-02] Revert when low level call fails instead of return false

Reverting on failure provides more immediate feedback about what went wrong. It bubbles up the error and interrupts the execution flow, making it clear to the caller that something unexpected occurred. Returning false may not adequately communicate the severity of the failure. If the caller doesn't check the return value properly, it might mistakenly assume that everything is okay when it's not.

```solidity
File : src/PrizeVault.sol

772:    function _tryGetAssetDecimals(IERC20 asset_) internal view returns (bool, uint8) {
773:        (bool success, bytes memory encodedDecimals) = address(asset_).staticcall(
774:            abi.encodeWithSelector(IERC20Metadata.decimals.selector)
775:        );
776:        if (success && encodedDecimals.length >= 32) {
777:            uint256 returnedDecimals = abi.decode(encodedDecimals, (uint256));
778:            if (returnedDecimals <= type(uint8).max) {
779:                return (true, uint8(returnedDecimals));
780:            }
781:        }
782:        return (false, 0);
783:    }

```

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L772C1-L783C6

## [L-03] Unsafe downcasting from uint256 to uint96 can be underflow (\*\*Note: Missed by bot-report)

The operation type(uint96).max - \_totalSupply is attempting to downcast a larger integer (uint256) into a smaller one (uint96). This is generally considered unsafe because it can lead to data loss or unexpected behavior if the value being downcasted is too large to fit into the smaller data type. If \_totalSupply is greater than type(uint96).max, subtracting it from type(uint96).max will result in an underflow.

```solidity
File : src/PrizeVault.sol

798:    function _twabSupplyLimit(uint256 _totalSupply) internal pure returns (uint256) {
799:        unchecked {
800:            return type(uint96).max - _totalSupply;
801:        }
802:    }

```

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVault.sol#L798C1-L802C6

## [L-04] Checks Effects Interactions pattern not followed

This issue can be mitigated by following the Checks-Effects-Interactions Pattern which suggests that a smart contract should first do the relevant Checks, then make the relevant internal changes to its state (Effects), and only then interact with other smart contracts which may well be malicious.

```solidity
File :

92:   function deployVault(
...
102:        PrizeVault _vault = new PrizeVault{
103:            salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))
104:        }(
105:            _name,
106:            _symbol,
107:            _yieldVault,
108:            _prizePool,
109:            _claimer,
110:            _yieldFeeRecipient,
111:            _yieldFeePercentage,
112:            YIELD_BUFFER,
113:            _owner
115        );
...
118:        IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER);
119:
120:        allVaults.push(_vault);
121:        deployedVaults[address(_vault)] = true;
122:
123:        emit NewPrizeVault(
124:            _vault,
125:            _yieldVault,
126:            _prizePool,
127:            _name,
128:            _symbol
129:        );
130:
131:        return _vault;
132:    }

```

https://github.com/code-423n4/2024-03-pooltogether/blob/main/pt-v5-vault/src/PrizeVaultFactory.sol#L92C5-L132C6
