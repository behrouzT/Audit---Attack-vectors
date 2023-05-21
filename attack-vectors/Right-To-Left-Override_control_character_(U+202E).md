<h1> Right-To-Left-Override (RTL) </h1>
 
The Right-To-Left-Override (RTL) unicode character (U+202E) control character vulnerability is one such classified weakness (although as of my last update in September 2021, this specific weakness isn't in the SWC Registry, but your question seems to suggest it has been added since then).

This vulnerability pertains to the way strings are rendered. The U+202E character is a special Unicode character that changes the direction of text from left-to-right (the standard for languages like English) to right-to-left (used in languages like Arabic and Hebrew). If used maliciously, this can result in a string being presented to the user in a way that obfuscates its true content.

For example, consider a contract that presents a confirmation message to the user before transferring funds. If a malicious actor manages to insert the U+202E character into the confirmation message, they could change the rendering of the message to mislead the user about the true amount to be transferred.
 
    solidity
    pragma solidity ^0.8.0;

    contract TransferFunds {
    function confirmTransfer(address recipient, uint256 amount) public view returns (string memory) {
        string memory confirmationMessage = string(abi.encodePacked("Confirm transfer of ", amount, " ETH to ", recipient));
        
        // Malicious actor inserts U+202E character to obfuscate the message
        confirmationMessage = string(abi.encodePacked(confirmationMessage, "\u202E"));
        
        return confirmationMessage;
    }
    }


In this contrived example, if `amount` was "1000" and `recipient` was "Alice", the user might see the message as "Alice to ETH 1000 of transfer Confirm", misleading the user about the true intent of the transaction.

This is why it's generally recommended to exclude the U+202E character from smart contract code: it rarely serves a legitimate purpose and can lead to serious security issues.



# Remediation

There are very few legitimate uses of the U+202E character. It should not appear in the source code of a smart contract.

- [SWC Registry: Right-To-Left-Override control character (U+202E)](https://swcregistry.io/docs/SWC-130)
