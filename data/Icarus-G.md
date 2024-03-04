# Constructors can be marked payable

Payable functions cost less gas to execute, since the compiler does not have to add extra checks to ensure that a payment wasn't provided. A constructor can safely be marked as payable, since only the deployer would be able to pass funds, and the project itself would not pass any funds.

in TwabERC20.sol and PrizeVault.sol and Claimable.sol