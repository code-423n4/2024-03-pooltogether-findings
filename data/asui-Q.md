1. Variables such as ```yieldFeePercentage```, ```liquidationPair``` and ```claimer``` should be immutable and functions like ```setClaimer```, ```setLiquidationPair``` and ```setYieldFeePercentage``` should be removed because it opens a way for malicious owners to rug users. 
I have taken out a message from the sponsor team in discord:
``` 
RaymondFam — Yesterday at 9:05 PM
This is intended as another option for prize vault promotion. If an org or prize vault creator wants to juice up their user's chance of winning, they could sponsor it. Doing this will increase the odds of winnings for the vault users. In other words, it's typically called by the prize vault owner to promote a campaign and attract more users to use the vault.
```
It is clear that other organizations or priceVault creators could use the PrizeVaultFactory to create a priceVault which will be controlled by them, with an underlying yeild source. And they could sponsor it to attract more users:
**`` Doing this will increase the odds of winnings for the vault users. In other words, it's typically called by the prize vault owner to promote a campaign and attract more users to use the vault. ``** 
This basically means that there could be vaults with users and an external owners(not controlled by the poolTogether team).
Normal users could be tricked into thinking their funds are safe since it uses poolTogether contracts but malicious owners can sponsor their vault to increase deposits and change the poolFeePercentage or the liquidationPair address as soon as users deposit and call ```transferTokensOut``` function with his address as the _receiver and rug the users.
This can lead to loss of funds for users and loss of users for the poolTogether protocol.
One recomendation would be to set the variables as immutable so that users can be sure their funds are safe even with vault owners other than the poolTogether team.