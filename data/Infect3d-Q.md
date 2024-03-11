
[L-01] Inconsistencies with IERC4626

from https://eips.ethereum.org/EIPS/eip-4626:

- `totalAsset` MUST be inclusive of any fees that are charged against assets in the Vault.
The fees are not accounted for as we can see here, they are comprised in the totalAssets while they should me substracted.
```solidity
File: pt-v5-vault\src\PrizeVault.sol
334:     /// @inheritdoc IERC4626
335:     /// @dev The latent asset balance is included in the total asset count to account for the "dust collection 
336:     /// strategy".
337:     function totalAssets() public view returns (uint256) {
338:         return yieldVault.convertToAssets(yieldVault.balanceOf(address(this))) + _asset.balanceOf(address(this)); 
339:     }	
340: 
```


[L-02] `PrizeVaultFactory::deployVault` uses an immutable variable to configure the yieldBuffer

As stated by the comment, the yield buffer should be configurable depending on the decimals, and the dollar value of the asset of the vault. This is not a big issue here as a `PrizVault` can also be deployed not using the factory if it needs a different yield buffer, but this means it will not be added to the `allVaults` array to be easily found by 3rd parties working with Pool Together.

It is probably a better idea to let the user configure it correctly, maybe still forcing a minimum value of 1e5 for it to be effetive.

```solidity
File: pt-v5-vault\src\PrizeVaultFactory.sol
059:     /// If the yield buffer is depleted on a vault, the vault will prevent any further 
060:     /// deposits if it would result in a rounding error and any rounding errors incurred by withdrawals
061:     /// will not be covered by yield. The yield buffer will be replenished automatically as yield accrues
062:     /// on deposits.
063:     uint256 public constant YIELD_BUFFER = 1e5;
...: /* removed unnecessary lines */
092:     function deployVault(
093:       string memory _name,
094:       string memory _symbol,
095:       IERC4626 _yieldVault,
096:       PrizePool _prizePool,
097:       address _claimer,
098:       address _yieldFeeRecipient,
099:       uint32 _yieldFeePercentage,
100:       address _owner
101:     ) external returns (PrizeVault) {
102:         PrizeVault _vault = new PrizeVault{
103:             salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++)) //@audit vulnerable to front-running
104:         }(
105:             _name,
106:             _symbol,
107:             _yieldVault,
108:             _prizePool,
109:             _claimer,
110:             _yieldFeeRecipient,
111:             _yieldFeePercentage,
112:             YIELD_BUFFER, 				//<@ should be configurable
113:             _owner
114:         );
...:
120:         allVaults.push(_vault);
121:         deployedVaults[address(_vault)] = true;
122:
```

[L-03] If the Yield Vault is Vulnerable to an Inflation Attack, funds can stay in Prize Vault on first depositor rather than being deposited in the Yield Vault

The PrizeVault should check that minted shares are non zero, in case of a vulnerable Yield Vault.
Otherwise, a unprotected Yield Vault would allow an attacker to inflate the share price . E.g: Solmate do not implement the decimal offset by default.
Another workaround would be to mint dead shares to the yield vault in PrizeVaultFactory::deployVault when deploying the PrizeVault if its a new one.

The steps are:
1) Alice deposit 1000 assets into the PrizeVault 
2) Bob front-run Victim by depositing 1 wei in YieldVault, then donating 1000 assets to YieldVault too
3) Alice tx is executed, 1000 assets are deposited into PrizeVault but not deposited to the Yield Vault.
4) Bob can take back its assets in the Yield Vault

In this state, the impact is really low as Alice can still get here asset backs from the Prize Vault by buring here shares, though this could potentially be escalated to something bigger.
