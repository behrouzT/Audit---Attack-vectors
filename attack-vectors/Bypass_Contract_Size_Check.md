"It is possible to create a contract with code size returned by extcodesize equal to 0. This can lead to bypass of 0-address check in a contract, which may lead to lost of fund or locking of funds."

In Ethereum, contracts can interact with other contracts on the blockchain. One common way to do this is by using the EXTCODESIZE opcode, which returns the size of the code of an external contract.

A vulnerability can arise when a contract is created with a code size that returns 0 for EXTCODESIZE. This can allow an attacker to bypass certain checks that rely on the existence of a contract at a certain address.

For example, some contracts may check whether a particular address is valid by verifying that EXTCODESIZE returns a non-zero value. If a contract with a code size of 0 is created at that address, the check may incorrectly pass, allowing an attacker to exploit the vulnerability.

This vulnerability can lead to lost or locked funds if the attacker is able to manipulate the contract's behavior in some way. For example, if the contract relies on external contracts for critical functionality such as sending or receiving funds, an attacker could potentially hijack the contract's behavior and steal or lock up funds.

To mitigate this vulnerability, it's important to ensure that contracts are not created with code size equal to 0, and to design contracts with robust checks and validation mechanisms that are not vulnerable to this kind of bypass. Additionally, developers should be aware of other types of attacks that can be used to exploit vulnerable contracts, and take steps to prevent and mitigate them as well.


- [Solidity By Example: Bypass Contract Size Check](https://solidity-by-example.org/hacks/contract-size/)
