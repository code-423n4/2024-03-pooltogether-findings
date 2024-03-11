[G-0] Floating pragma leads to compatibility issues 

Description : The exact compiler version used for development might not be preserved, potentially leading to unexpected changes in the code's execution or security vulnerabilities. Additionally, relying on the floating pragma could hinder code auditability and reproducibility, making it challenging to ensure the stability and reliability of smart contracts over time. 

[G-1] Bad convention of state variables

Description :  In TwabERC20.sol  , declaration of immutable variables like that is not a recommended approach because it leads to confusion for other developers or auditors who are going to review this contract, state variables and immutable variables are going to store directly on blockchain so we need to handle them properly, we better to declare them uniquely from other local variables.

TwabController public immutable twabController;

Recommended Mitigation : 

TwapController public immutable i_twabController;

[G-2] Avoid Unnecessary checks to save gas 

Description : In PriceVault.sol contract, there is a zero address check for the parameter yieldVault_  of type ERC46426 within the constructor. But it is not necessary to check for zero address for the contracts, because if they give zero address, and trying to call any function from that pre-designed contract,the solidity simply reverts it.

Recommended Mitigation :  So to save gas, we can ignore that check.

if (address(yieldVault_) == address(0)) revert YieldVaultZeroAddress();

just ignore this line from the codebase.

[M-0] Not updating the mapping leads to unexpected behaviour

Description : In PrizeVaultFactory.sol, in deployVault function, they are using deployerNonces mapping, but by the end of the function they are updating the value in the deployerNonces mapping, it can leads to unexpected functioning of this function when we call this function again.

Recommended Mitigation : Just increment the deployerNonces for that particular caller.

### Time spent:
6 hours