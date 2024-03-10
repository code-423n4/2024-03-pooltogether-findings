# [NC/G-1] - PrizeVault::totalYieldBalance can be removed
- `totalYieldBalance` is not used in the contract instead `_totalYieldBalance` is used directly
```
    //@audit-info function not used anywhere in the contract
    //_totalYieldBalance is used everywhere directly
    function totalYieldBalance() public view returns (uint256) {
        return _totalYieldBalance(totalAssets(), totalDebt());
    }
```
# [L-1] - Claimable::Constructor has no checks for `claimer_` address
```
    constructor(PrizePool prizePool_, address claimer_) {
        if (address(prizePool_) == address(0)) revert PrizePoolZeroAddress();
        prizePool = prizePool_;
        //@audit no checks for claimer_ address and after deployment claimer can not be changed
        _setClaimer(claimer_);
    }
```
# [L-2] - No checks for already deployed vault in `PrizeVaultFactory.sol`
```
    function deployVault(
      string memory _name,
      string memory _symbol,
      IERC4626 _yieldVault,
      PrizePool _prizePool,
      address _claimer,
      address _yieldFeeRecipient,
      uint32 _yieldFeePercentage,
      address _owner
    ) external returns (PrizeVault) {
        //@audit-info no checkes of already deployed
        PrizeVault _vault = new PrizeVault{
            salt: keccak256(abi.encode(msg.sender, deployerNonces[msg.sender]++))
        }(
            _name,
            _symbol,
            _yieldVault,
            _prizePool,
            _claimer,
            _yieldFeeRecipient,
            _yieldFeePercentage,
            YIELD_BUFFER,
            _owner
        );

        // A donation to fill the yield buffer is made to ensure that early depositors have
        // rounding errors covered in the time before yield is actually generated.
        IERC20(_vault.asset()).transferFrom(msg.sender, address(_vault), YIELD_BUFFER);

        allVaults.push(_vault);
        deployedVaults[address(_vault)] = true;

        emit NewPrizeVault(
            _vault,
            _yieldVault,
            _prizePool,
            _name,
            _symbol
        );

        return _vault;
    }
```