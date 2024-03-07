699 prizevault.sol Not using the named return variables when a function returns,waste gas.
Recommendation
Return transferyieldout
753 & 759 Using modifier rather than invoking functions to perform checks save gas.
Recommendation
    function setYieldFeePercentage(uint32 _yieldFeePercentage) external onlyOwner setyieldFeepercentage {