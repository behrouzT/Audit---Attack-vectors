As written in the solidity documentation, the low-level functions call, delegatecall and staticcall return true as their first return value if the account called is non-existent, as part of the design of the EVM. Account existence must be checked prior to calling if needed.

This can potentially be exploited by attackers who may deliberately call non-existent accounts in order to trick the system into believing that a transaction was successful. This could lead to unintended consequences such as loss of funds or unauthorized changes to the smart contract's state.

For example, let's say there is a smart contract that uses the call function to send Ether to a specific account. An attacker could create a fake account with a similar address and trick the contract into sending the Ether to the fake account instead of the intended recipient. Since the call function returns true even if the account does not exist, the smart contract would not realize that the transaction failed.

To prevent such attacks, it's important to check the existence of the account before calling these low-level functions. This can be done by using the address.balance property or the address.exists() function.

here is an example Solidity code snippet that demonstrates how to check the existence of an account before making a call using the address.exists() function:

    pragma solidity ^0.8.0;

    contract AccountChecker {
    function transfer(address _to, uint256 _amount) external {
        // Check if the account exists before making the call
        if (address(_to).exists()) {
            // Make the call to transfer the funds
            (bool success, ) = _to.call{value: _amount}("");
            require(success, "Transfer failed.");
        } else {
            // Handle the case where the account does not exist
            revert("Account does not exist.");
        }
    }
    }


In this example, the transfer function takes two arguments: the address of the recipient account (_to) and the amount to be transferred (_amount).

Before making the call to transfer the funds, the function first checks if the account exists by calling the exists() function on the address. If the account exists, the function makes the call using the _to address and the _amount value. If the call is successful, the function proceeds normally. If the call fails, the function will revert with an error message.

If the account does not exist, the function will revert with an error message stating that the account does not exist. This prevents the contract from accidentally transferring funds to a non-existent account.

## Remediation

Check before any low-level call that the address actually exists, for example before the low level call in the callERC20 function you can check that the address is a contract by checking its code size.
