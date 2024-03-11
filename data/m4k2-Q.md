# LOW INFO vulnerability report

LOW / INFO vulnerabilities are not considered as a threat to the protocol. They are just informational and can be considered as suggestions for improvement.

## Issue 1: Recover ERC20 Tokens Sent by Mistake

### Explanation:
There is a need to write a function that allows the recovery of ERC20 tokens that were mistakenly sent to the contract. Currently, there is no mechanism in place to handle such situations.

### Remediation:
Implement a function that allows the contract owner to recover ERC20 tokens sent by mistake. This function should have proper authorization checks and ensure that only the rightful owner can initiate the recovery process.

## Issue 2: Arbitrary Decimal Setting

### Explanation:
If the decimal value for an asset is not fetched successfully, the code arbitrarily sets the decimal value to 18. This can lead to incorrect calculations and potential issues.

### Remediation:
Instead of arbitrarily setting the decimal value to 18, the code should revert the transaction if the decimal value cannot be fetched successfully. This will prevent any potential miscalculations and ensure accurate processing of the asset.

## Issue 3: Launching Prize Vault without Set Decimals

### Explanation:
The code currently launches the prize vault even if the decimals for the underlying asset are not set. This can result in unexpected behavior and potential issues.

### Remediation:
Implement a function that allows the contract owner to set the decimals for the underlying asset. Before launching the prize vault, ensure that the decimals are properly set. If the decimals are not set, the launch should be prevented to avoid any undesired behavior.

