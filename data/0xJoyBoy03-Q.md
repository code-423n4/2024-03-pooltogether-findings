# Low
### [L-1] `PrizeVualt::maxDeposit` function will revert in some case

**Description** The nature of the `maxDeposit` function based on its natspac must not revert but a `yieldVault` contract made by a user could intentionally or unintentionally override its own `YieldVault::maxDeposit` function to some condition that will revert or it could just maliciously get reverted. the user could create and init this with the `PrizeVaultFactory` contract.

```java
    // Portion of the `maxDeposit` function of the PrizeVault contract
        function maxDeposit(address) public view returns (uint256) {
        uint256 _totalSupply = totalSupply();
        uint256 totalDebt_ = _totalDebt(_totalSupply);
        if (totalAssets() < totalDebt_) return 0;

        uint256 twabSupplyLimit_ = _twabSupplyLimit(_totalSupply);
        uint256 _maxDeposit;
        uint256 _latentBalance = _asset.balanceOf(address(this));
        // @audit-low this func should not revert but a malicious `yieldVault` can revert this
        uint256 _maxYieldVaultDeposit = yieldVault.maxDeposit(address(this));
```

**Impact** It just breaks the functionalities of the protocol based on its natspac that says in the `IERC4626` contract: "*MUST NOT revert.*"
but of course, it doesn't lose value and only the contract fails to deliver promised returns

**Proof of Concept:** 
1. A user could create a custom `yieldVault` contract to something like this:

```java
    contract YieldVault is ERC4626Mock {
    using Math for uint256;

    constructor(address _asset, string memory _name, string memory _symbol) ERC4626Mock(_asset) {}
    // Note that this is just an example
    //The user could unintentionally make some conditions for
    // its own `maxDeposit` function to get reverted
    function maxDeposit(address) public view virtual override returns (uint256) {
        revert("Max Deposit reverted!!!");
    }
}
```


2. Include this test in `PrizeVaultFactory.t.sol` :

```java
        function test_maxDepositCanRevert() public {
        PrizeVault _vault;

        asset.mint(address(this), yieldBuffer);
        asset.approve(address(vaultFactory), yieldBuffer);

        _vault = vaultFactory.deployVault(
            name,
            symbol,
            yieldVault, // The user's YieldVault contract which we certainly imported
            PrizePool(address(prizePool)),
            claimer,
            address(this),
            yieldFeePercentage,
            owner
        );
        address anyUser = makeAddr('anyUser');
        vm.startPrank(anyUser);
        vm.expectRevert("Max Deposit reverted!!!");
        _vault.maxDeposit(anyUser);
        vm.stopPrank();
    }
```


3. *Logs*:
   ```bash
    Ran 1 test for test/unit/PrizeVault/PrizeVaultFactory.t.sol:PrizeVaultFactoryTest
    [PASS] test_maxDepositCanRevert() (gas: 3867603)
    Test result: ok. 1 passed; 0 failed; 0 skipped; finished in 4.26ms

    Ran 1 test suite in 4.26ms: 1 tests passed, 0 failed, 0 skipped (1 total tests)
   ```


**Recommend Mitigation** Change the natspac of the `PrizeVault::maxDeposit` function to something like this :

```diff

+   /// @note this function could revert
    function maxDeposit(address) public view returns (uint256) {
```

