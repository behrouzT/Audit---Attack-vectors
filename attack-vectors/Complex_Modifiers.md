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



- [Consensys Best Practices: Use Modifiers Only for Assertions](https://consensys.github.io/smart-contract-best-practices/recommendations/#use-modifiers-only-for-assertions)
