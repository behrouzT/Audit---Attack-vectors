Solidity supports function types. That is, a variable of function type can be assigned with a reference to a function with a matching signature. The function saved to such variable can be called just like a regular function.

For example, a function add that takes two integer arguments and returns their sum can be assigned to a variable of function type as follows:


    function add(uint x, uint y) public pure returns (uint) {
    return x + y;
    }

    function callAdd() public {
    function (uint, uint) pure returns (uint) myFunc = add;
    uint result = myFunc(2, 3);
    // result will be 5
    }

In this example, the callAdd function assigns the add function to a variable called myFunc that has the same signature (i.e., two uint arguments and a uint return value) as add. The myFunc variable can then be called just like a regular function, passing in two arguments and receiving a return value.This can be dangerous because it allows an attacker to execute arbitrary code instructions and potentially steal funds or cause other malicious actions.

The problem arises when a user has the ability to arbitrarily change the function type variable and thus execute random code instructions. As Solidity doesn't support pointer arithmetics, it's impossible to change such variable to an arbitrary value. However, if the developer uses assembly instructions, such as `mstore` or assign operator, in the worst case scenario an attacker is able to point a function type variable to any code instruction, violating required validations and required state changes.

- [SWC Registry: Arbitrary Jump with Function Type Variable](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-127)
