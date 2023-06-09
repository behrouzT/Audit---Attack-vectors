# Access Control

There are a number of common mistakes relating to access control.

## Default Visibility

The default visibility of solidity functions/state variables is `public`. Developers may forget to specify the visibility for a function/state variable that is intended to be private.

## Authentication With tx.origin

Sometimes developer use tx.origin instead of msg.sender. Tx.origin returns the address of the account that originally sent the call. Use this varible for authenticating leaves contract vulnerable to a phising-like attack.

When developing smart contracts on the Ethereum blockchain, it is important to properly authenticate and verify the identities of users interacting with the contract. One way to do this is by using the "msg.sender" variable, which returns the address of the account that directly called the contract function.

However, some developers may mistakenly use the "tx.origin" variable instead of "msg.sender" to authenticate users. "tx.origin" returns the address of the account that originally initiated the transaction, even if the transaction was made through another contract.

Using "tx.origin" for authentication can leave the contract vulnerable to a phishing-like attack. In such an attack, a malicious contract could use a legitimate contract as a middleman to call the vulnerable contract with a different "tx.origin" address, making it appear as if the call was coming from a trusted source.

For example, let's say a user wants to interact with a contract that requires authentication through "tx.origin". A malicious contract could deceive the user by impersonating the authentic contract and passing on the user's transaction with a different "tx.origin" address, thereby gaining unauthorized access to the vulnerable contract.

To avoid this vulnerability, it is recommended that developers always use "msg.sender" instead of "tx.origin" for user authentication in smart contracts.
here is an example code snippet that demonstrates the difference between using "msg.sender" and "tx.origin" in a smart contract:


----------------------------------------------------------------------------------------
            
    pragma solidity ^0.8.0;

    contract AuthenticationExample {

    address public owner;

    constructor() {
        owner = msg.sender;
    }

    function doSomething() public {
        require(msg.sender == owner, "Unauthorized user");
        // do something
    }

    function doSomethingUnsafe() public {
        require(tx.origin == owner, "Unauthorized user");
        // do something
    }
--------------------------------------------------------------------------------------


In this example, the contract has an "owner" address variable that is initialized to the address of the contract deployer, which is stored in the "msg.sender" variable in the constructor.

The "doSomething()" function uses "msg.sender" to authenticate that the caller of the function is the owner of the contract. If the caller is not the owner, the function will fail with an "Unauthorized user" error message.

On the other hand, the "doSomethingUnsafe()" function uses "tx.origin" to authenticate the user. This function is vulnerable to the phishing attack described earlier because it assumes that the "tx.origin" variable is always trustworthy. If a malicious contract calls "doSomethingUnsafe()" with a different "tx.origin" address, the contract will think the malicious contract is the owner and allow it to execute the function.

Therefore, it is always recommended to use "msg.sender" instead of "tx.origin

- [Sigmaprime: tx.origin Authentication](https://blog.sigmaprime.io/solidity-security.html#tx-origin)

## Signature Verification

It is a common pattern for smart contract systems to allow users to sign messages off-chain instead of directly requesting users to do an on-chain transaction because of the flexibility and increased transferability that this provides. Smart contract systems that process signed messages have to implement their own logic to recover the authenticity from the signed messages before they process them further. A limitation for such systems is that smart contracts can not directly interact with them because they can not sign messages. Some signature verification implementations attempt to solve this problem by assuming the validity of a signed message based on other methods that do not have this limitation. An example of such a method is to rely on msg.sender and assume that if a signed message originated from the sender address then it has also been created by the sender address. This can lead to vulnerabilities especially in scenarios where proxies can be used to relay transactions.

The SWC (Smart Contract Weakness Classification) Registry is a platform that is designed to identify and categorize different types of security weaknesses that exist in smart contracts. One particular security issue that has been identified in the SWC Registry is the lack of proper signature verification in smart contracts.

Signature verification is an important security mechanism that is used to ensure the authenticity of transactions. In the context of smart contracts, signature verification is used to verify that a particular transaction has been authorized by the correct party. Without proper signature verification, unauthorized parties could potentially carry out transactions on the smart contract, which could lead to financial losses or other negative consequences.

The lack of proper signature verification in smart contracts can be attributed to a number of factors. One common issue is that developers may not have a clear understanding of how to properly implement signature verification in their smart contracts. Additionally, there may be a lack of clear standards or best practices for implementing signature verification, which can make it difficult for developers to know what approach to take.

To address this issue, the SWC Registry recommends that developers take steps to ensure proper signature verification in their smart contracts. This can include implementing multi-factor authentication mechanisms, such as requiring users to enter a password or use a hardware device to sign transactions. Additionally, developers should be sure to follow established best practices and standards for implementing signature verification in their smart contracts.

Overall, the lack of proper signature verification in smart contracts is a serious security issue that needs to be addressed. By following established best practices and taking steps to implement multi-factor authentication mechanisms, developers can help to ensure the security and integrity of their smart contracts.

- [SWC Registry: Lack of Proper Signature Verification](https://swcregistry.io/docs/SWC-122)

### Remediation

It is not recommended to use alternate verification schemes that do not require proper signature verification through `ecrecover()`.

## Unprotected Ether Withdrawal

Due to missing or insufficient access controls, malicious parties can withdraw some or all Ether from the contract account.

This bug is sometimes caused by unintentionally exposing initialization functions. By wrongly naming a function intended to be a constructor, the constructor code ends up in the runtime byte code and can be called by anyone to re-initialize the contract.

An example of unintentionally exposing an initialization function in a smart contract might look something like this:

    pragma solidity ^0.8.0;

    contract MyContract {
    address owner;
    uint public balance;

    function MyContract() public {
        owner = msg.sender;
        balance = 100 ether;
    }

    function withdraw() public {
        require(msg.sender == owner);
        owner.transfer(balance);
        balance = 0;
    }
    }

In this example, the intention is to have a constructor function that sets the contract owner and initializes the balance to 100 ether. However, due to an unintentional mistake, the function has been named MyContract instead of constructor. This means that the function can be called by anyone at any time, allowing anyone to re-initialize the contract and potentially steal the ether stored in the contract.

To fix this vulnerability, the function should be renamed to constructor so that it is only called once during contract deployment, and the access control for the withdraw function should be reviewed to ensure that only the owner can withdraw funds from the contract.


- [SWC Registry: Unprotected Ether Withdrawal](https://swcregistry.io/docs/SWC-105)

## Unprotected SELFDESTRUCT Instruction

Due to missing or insufficient access controls, malicious parties can self-destruct the contract.

- [SWC Registry: Unprotected SELFDESTRUCT](https://swcregistry.io/docs/SWC-106)

## Missed Modifier

It is important to have access control validations on critical functions that execute actions like modifying the owner, transfer of funds and tokens, pausing and unpausing the contracts, etc. Missing validations either in the modifier or inside require or conditional statements will most probably lead to compromise of the contract or loss of funds.

## Incorrect Modifier Names

Due to the developer’s mistakes and spelling errors, it may happen that the name of the modifier or function is incorrect than intended. Malicious actors may also exploit it to call the critical function without the modifier, which may lead to loss of funds or change of ownership depending on the function’s logic.

## Overpowered Roles

Allowing users to have overpowered roles may lead to vulnerabilities. The practice of least privilege must always be followed in assigning privileges.
