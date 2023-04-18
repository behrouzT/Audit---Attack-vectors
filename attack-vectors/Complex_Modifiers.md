Use modifiers only for input validation with `require`. Modifiers should not contain any substantive logic, because that logic will be executed before any input validation done at the start of function bodies.


In Solidity, modifiers are used to add extra functionality to a function or restrict its access. They are typically used to check whether the caller of the function is authorized to execute the function or not.

Modifiers are usually defined as functions that return nothing (void), but they are invoked with a modifier keyword instead of function. When a function with a modifier is called, the modifier code is executed first, and then the function code runs.

It is important to note that modifiers are executed before any input validation in the function body. This means that if a modifier contains substantive logic, it will be executed before the input validation is performed. This can lead to unexpected behavior and security vulnerabilities in the code.

Therefore, it is recommended to use modifiers only for input validation with require, and not to include any substantive logic in the modifier. This ensures that the input validation is performed before any other code is executed, making the code more secure and less prone to unexpected behavior.

Here's an example of using a modifier for input validation with require, without any substantive logic:


    pragma solidity ^0.8.0;

    contract Example {
    uint public value;

    modifier validValue(uint newValue) {
        require(newValue >= 0, "Value must be non-negative");
        _;
    }

    function setValue(uint newValue) public validValue(newValue) {
        value = newValue;
    }
    }



In this example, the validValue modifier is used to validate that the input newValue is non-negative. If the validation fails (i.e. the require statement evaluates to false), then the function call will revert and any state changes made up to that point will be undone. If the validation passes, then the function body will be executed as normal (_ is a placeholder that gets replaced with the function body code). The modifier itself contains no substantive logic beyond the input validation.


 here's an example of a vulnerable use of modifiers:
 
     contract VulnerableContract {
     uint public counter;

    modifier onlyPositive(uint num) {
    if (num < 0) {
      counter++;
      revert("Number must be positive.");
    }
    _;
     }

     function addNumber(uint num) public onlyPositive(num) {
    counter += num;
     }
    }



In this example, the onlyPositive modifier not only checks that the input is positive, but it also increments a counter variable. This is problematic because the modifier is executed before the function body, so the counter variable will be incremented even if the input is invalid and the function call is reverted.

This creates a vulnerability because an attacker could repeatedly call the addNumber function with negative numbers, causing the counter variable to increase without actually modifying the state of the contract. This could potentially lead to a denial-of-service attack by consuming the gas limit of the contract.

To fix this, the onlyPositive modifier should only contain input validation with a require statement, and any other logic should be moved into the function body.


- [Consensys Best Practices: Use Modifiers Only for Assertions](https://consensys.github.io/smart-contract-best-practices/recommendations/#use-modifiers-only-for-assertions)
