# 1. Potential violation of ERC-20 standard in TWABERC20 upon self transfer 

Lines of code*

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/TwabERC20.sol#L100

## Impact
According to the ERC20 standard, upon transfer, the function should revert if the caller's account balance doesn't have enough to spend.

>  transfer
> Transfers _value amount of tokens to address _to, and MUST fire the Transfer event. The function SHOULD throw if the message caller’s account balance does not have enough tokens to spend.

And in during the transfer of the TwabERC20, the internal transfer function is invoked.

```
    function _transfer(address _from, address _to, uint256 _amount) internal virtual override {
        twabController.transfer(_from, _to, SafeCast.toUint96(_amount));
        emit Transfer(_from, _to, _amount);
    }
```

The function then makes a call to the twabController's transfer function.

```
  function transfer(address _from, address _to, uint96 _amount) external {
    if (_to == address(0)) {
      revert TransferToZeroAddress();
    }
    _transferBalance(msg.sender, _from, _to, _amount);
  }
```
which calls the internal `_transferBalance` function.

```
  function _transferBalance(address _vault, address _from, address _to, uint96 _amount) internal {
    if (_to == SPONSORSHIP_ADDRESS) {
      revert CannotTransferToSponsorshipAddress();
    }

    if (_from == _to) {
      return;
    }

    // If we are transferring tokens from a delegated account to an undelegated account
    address _fromDelegate = _delegateOf(_vault, _from);
    address _toDelegate = _delegateOf(_vault, _to);
    if (_from != address(0)) {
      bool _isFromDelegate = _fromDelegate == _from;

      _decreaseBalances(_vault, _from, _amount, _isFromDelegate ? _amount : 0);

...
  }
```

As can be seen from the function, upon self transfer (when from == to), the function returns, skipping a potential balance check. This means that a user could potentially transfer more than his account balance to himself, and the function would return successfully, albeit no funds are transferred.

This breaks the standard by not reverting.

## Recommended Mitigation Steps
Include a balance check in the the TwabERC20 `_transfer` function and revert if amount being transferred is more than user's balance.

***
# 2. Contrary to the comment, vaults cannot be deployed without claimer

Lines of code* 
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVaultFactory.sol#L80

## Impact
According the the comment above the deployVault function, and possibly, the devs intention, The vaults can be initially deployed without a claimer address, i.e 0

>    /// @dev `claimer` can be set to address zero if none is available yet.

```
    function deployVault(
      string memory _name,
      string memory _symbol,
      IERC4626 _yieldVault,
      PrizePool _prizePool,
      address _claimer, //@note can be 0
      address _yieldFeeRecipient,
      uint32 _yieldFeePercentage,
      address _owner
    ) external returns (PrizeVault) {
```

As such, in the prize vault's contructor, the declared claimer address is passed into the Claimable contract for deployment. 

```
    constructor(
        string memory name_,
        string memory symbol_,
        IERC4626 yieldVault_,
        PrizePool prizePool_,
        address claimer_, //@note still 0
        address yieldFeeRecipient_,
        uint32 yieldFeePercentage_,
        uint256 yieldBuffer_,
        address owner_
    ) TwabERC20(name_, symbol_, prizePool_.twabController()) Claimable(prizePool_, claimer_) Ownable(owner_) {
```

As it flow continues, in the Claimable contract constructor, the claimer is declared and the `_setClaimer` function is called. 

```
    constructor(PrizePool prizePool_, address claimer_) {
        if (address(prizePool_) == address(0)) revert PrizePoolZeroAddress();
        prizePool = prizePool_;
        _setClaimer(claimer_);
    }
```

The issue occurs when the the `setClaimer` function is called as it contains a 0 address check, hence will cause deployment to revert.

```
    function _setClaimer(address _claimer) internal {
        if (_claimer == address(0)) revert ClaimerZeroAddress();
        claimer = _claimer;
        emit ClaimerSet(_claimer);
    }
```

Hard to infer if this is an error in the comments or a real issue, as claimer contracts can also be [deployed](https://dev.pooltogether.com/protocol/guides/creating-vaults#deploying-a-claimer-contract) through the claimer factory, pointing out that users have the choice of deploying either the vault or the claimer first.

## Recommended Mitigation Steps
1. Update the comment or documentation, to correct for this error.
2. If it's truly desired that vaults can be deployed without claimers, then remove the check for 0 address from the `_setClaimer` 
```
    function _setClaimer(address _claimer) internal {
        claimer = _claimer;
        emit ClaimerSet(_claimer);
    }
```
and move it to the `setClaimer` function in the PrizeVault contract.

```
    function setClaimer(address _claimer) external onlyOwner {
        if (_claimer == address(0)) revert ClaimerZeroAddress();
        _setClaimer(_claimer);
    }
```

This way, the vaults can be deployed without vaults and the owner can always set the claimer later.
***


# 3. Add checks for same parameter updates

A check regarding whether the current value and the new value are the same should be added.

Lines of code* 
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L958

```
    function _setYieldFeeRecipient(address _yieldFeeRecipient) internal {
        yieldFeeRecipient = _yieldFeeRecipient;
        emit YieldFeeRecipientSet(_yieldFeeRecipient);
    }
```

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L947

```
    function _setYieldFeePercentage(uint32 _yieldFeePercentage) internal {
        if (_yieldFeePercentage > MAX_YIELD_FEE) {
            revert YieldFeePercentageExceedsMax(_yieldFeePercentage, MAX_YIELD_FEE);
        }
        yieldFeePercentage = _yieldFeePercentage;
        emit YieldFeePercentageSet(_yieldFeePercentage);
    }
```
https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L742

```
    function setLiquidationPair(address _liquidationPair) external onlyOwner {
        if (address(_liquidationPair) == address(0)) revert LPZeroAddress();

        liquidationPair = _liquidationPair;

        emit LiquidationPairSet(address(this), address(_liquidationPair));
    }
```

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/abstract/Claimable.sol#L128

```
    function _setClaimer(address _claimer) internal {
        if (_claimer == address(0)) revert ClaimerZeroAddress();
        claimer = _claimer;
        emit ClaimerSet(_claimer);
    }
```
