## Use named returns for local variables of pure functions where it is possible

## Description

* Streamline return values: A simple yet effective optimisation technique is to name the return value in a function, eliminating the need for a separate local variable. For instance, in a function that calculates a product, you can directly name the return value, streamlining the process


## Proof of Concept

```
library NamedReturnArithmetic {

    function sum(uint256 num1, uint256 num2) internal pure returns(uint256 theSum){
        theSum = num1 + num2;
    }
}
contract NamedReturn {
    using NamedReturnArithmetic for uint256;
    uint256 public stateVar;
    function add2State(uint256 num) public {
        stateVar = stateVar.sum(num);
    }
}

######RUN######

test for test/NamedReturn.t.sol:NamedReturnTest
[PASS] test_Increment() (gas: 27613)

------------------------------------------------------------------------

library NoNamedReturnArithmetic {

    function sum(uint256 num1, uint256 num2) internal pure returns(uint256){
        return num1 + num2;
    }
}
contract NoNamedReturn {
    using NoNamedReturnArithmetic for uint256;
    uint256 public stateVar;
    function add2State(uint256 num) public {
        stateVar = stateVar.sum(num);
    }
}

######RUN######

test for test/NoNamedReturn.t.sol:NamedReturnTest
[PASS] test_Increment() (gas: 27639)

```

There are 5 instances of this issue:


https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L337

https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L447


https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L798


https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/src/PrizeVault.sol#L808


https://github.com/code-423n4/2024-03-pooltogether/blob/480d58b9e8611c13587f28811864aea138a0021a/pt-v5-vault/test/unit/HookManager.t.sol#L56

```