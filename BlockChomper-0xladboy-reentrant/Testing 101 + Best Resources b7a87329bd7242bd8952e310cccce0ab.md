# Testing 101 + Best Resources

  * [Introduction](#introduction)
  * [Validator Node Testing](#validator-node-testing)
    + [Guide to Unit Testing with Zeta-Chain](#guide-to-unit-testing-with-zeta-chain)
    + [Viewing the Unit Test Coverage Report Online](#viewing-the-unit-test-coverage-report-online)
    + [Integration Testing Guide](#integration-testing-guide)
  * [Example Testing Strategy and Test Case](#example-testing-strategy-and-test-case)
    + [Inbound Transaction Observation Test Cases](#inbound-transaction-observation-test-cases)
    + [Additional Considerations](#additional-considerations)
    + [Potential Attack Vectors](#potential-attack-vectors)
    + [Resources](#resources)

![Untitled](Testing%20101%20+%20Best%20Resources%20b7a87329bd7242bd8952e310cccce0ab/Untitled.png)

## Introduction

This Testing 101 primer will give a competent security researcher a crucial head start on testing the ZetaChain project. We deep dive into Validator Node testing considerations and then go on to outline a robust wider testing strategy. We also outline potential attack vectors and list relevant and valuable resources available to wardens.

This Alpha content is crafted one of the top security researchers active in Code4rena (0xladboy) and researchers would do well to take heed of the recommendations/advice given here!

## **Validator Node Testing**

**Prerequisites & Setup Instructions:**

---

1. **This tutorial** assumes that readers have a foundational understanding of 
    - Go programming,
    - Bash scripting,
    - Docker.
2. **Operating System:**
    - The test command is optimized for Linux-based systems.
    - If you're on Windows, please utilize the Windows Subsystem for Linux (WSL).
3. **Required Software:**
    - Install **Golang 1.20**.
        - [Download Golang here](https://go.dev/dl/).
        - Verify your installation by running the command:
            
            ```
            go version
            ```
            

---

![Screenshot 2023-10-29 at 5.16.17 PM.png](Testing%20101%20+%20Best%20Resources%20b7a87329bd7242bd8952e310cccce0ab/Screenshot_2023-10-29_at_5.16.17_PM.png)

- docker [installation]([https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)) link and set up
- make sure to run “docker version” to confirm the docker installation
    
    ![Screenshot 2023-10-29 at 5.17.36 PM.png](Testing%20101%20+%20Best%20Resources%20b7a87329bd7242bd8952e310cccce0ab/Screenshot_2023-10-29_at_5.17.36_PM.png)
    
- Clone **Zeta-Chain Node Repository and follow the [readme](https://github.com/zeta-chain/node/tree/ea9ba1271de86bc06e58d51a0de1a29650908ef5) to build the node**
    - **Repository URL**: [Zeta-Chain Node on GitHub](https://github.com/zeta-chain/node)
    - **Branch**: **`developer`**
    - **Commit Hash**: **`ea9ba1271de86bc06e58d51a0de1a29650908ef5`**
    - command:
        
        ```bash
        git clone https://github.com/zeta-chain/node
        ```
        

### **Guide to Unit Testing with Zeta-Chain:**

---

1. The [Makefile](https://github.com/zeta-chain/node/blob/develop/Makefile#L61C24-L61C24) within the validator node provides concise aliases for both building and executing tests
2. **File Location:**
    
    Unit testing files are located adjacent to their respective code files.
    
    **Example:** Within the directory
    [https://github.com/zeta-chain/node/tree/develop/common](https://github.com/zeta-chain/node/tree/develop/common), you'll notice the unit test for `address.go` is named `address_test.go`.
    
3. **Running Unit Tests:**
    
    For enhanced debugging and testing visibility, consider modifying the test command in the `Makefile` to include the verbose flag, allowing it to display detailed outputs in the console.
    
    Change this line in the [Zeta-Chain Node's Makefile](https://github.com/zeta-chain/node/blob/ea9ba1271de86bc06e58d51a0de1a29650908ef5/Makefile#L61C24-L61C24):
    
    ```bash
    @go test ${TEST_BUILD_FLAGS} ${TEST_DIR}
    ```
    
    to:
    
    ```bash
    @go test ${TEST_BUILD_FLAGS} -v ${TEST_DIR}
    ```
    

To execute all unit tests, use the command:

```bash
make run-test
```

1. G**enerating Test Coverage Reports:**
    
    To run unit tests and generate a test coverage report, use:
    
    ```bash
    make coverage-report
    ```
    
    This will create an output file named `coverage.html`. You can then view the coverage report using any browser by opening the file `coverage.html`
    

---

![Screenshot 2023-10-29 at 5.08.55 PM.png](Testing%20101%20+%20Best%20Resources%20b7a87329bd7242bd8952e310cccce0ab/Screenshot_2023-10-29_at_5.08.55_PM.png)

1. **How to modify and run individual unit test**
    
    If we want to run all unit tests under a folder, we can run the command:
    

```bash
go test -tags TESTNET,pebbledb,ledger -v -run Test ./common
```

![Screenshot 2023-10-30 at 9.00.37 AM.png](Testing%20101%20+%20Best%20Resources%20b7a87329bd7242bd8952e310cccce0ab/Screenshot_2023-10-30_at_9.00.37_AM.png)

If you want to run a specific test within that file, you can use the `-run` flag followed by the test function name or a regex pattern that matches the test function name.

If you are only interested in running one unit test, for example, if you want to modify and run the test [TestTrueBitcoinHeader](https://github.com/zeta-chain/node/blob/ea9ba1271de86bc06e58d51a0de1a29650908ef5/common/headers_test.go#L21), you can add a print statement within the test to ensure that it's being executed. Modify the test function in `headers_test.go` to include:

```go
import "fmt"

func TestTrueBitcoinHeader(t *testing.T) {
    fmt.Println("Running TestTrueBitcoinHeader...")
    // Rest of the test code...
}

```

Then run:

```bash
go test -tags TESTNET,pebbledb,ledger -v -run TestTrueBitcoinHeader ./common

```

![Screenshot 2023-10-30 at 9.06.56 AM.png](Testing%20101%20+%20Best%20Resources%20b7a87329bd7242bd8952e310cccce0ab/Screenshot_2023-10-30_at_9.06.56_AM.png)

---

### **Viewing the Unit Test Coverage Report Online:**

You can conveniently access the unit test coverage report online. For example, the file **`zetacore/common/ethereum/proof.go`** has a test coverage of 50%. We’ve hosted the unit test coverage report and you can [view this specific coverage report here](https://deft-duckanoo-3b3f15.netlify.app/#file52).

**Importance of Test Coverage Reports:**

Coverage reports are invaluable as they provide insights into:

1. **Well-Tested Areas:** This helps in leveraging the existing tests to complete Proof of Concepts (POCs) and exploits.
2. **Areas Requiring Attention:** Highlighting less-tested portions of the code, enabling focused efforts to identify potential issues.

---

### **Integration Testing Guide**

<aside>
💡  Detailed steps for setting up the integration test environment are also available in the [README](https://github.com/zeta-chain/node/blob/develop/contrib/localnet/README.md).

</aside>

**About the Smoke Test**: You will likely run into the Smoke Test in the `zeta-chain/node` repo.  This is essentially a set of sanity integration tests. The intent is to have a fully automated setup, necessitating only a few initial steps for image building and initiating Docker Compose.

What follows are steps for getting acquainted with the integration test suite. These tests exercise the major components of the ZetaChain codebase.  The integration testing code can be found at this [link](https://github.com/zeta-chain/node/tree/develop/contrib/localnet/orchestrator/smoketest).

During the Smoke Test, a network is established comprising of:

- 2 ZetaChain nodes
- 2 ZetaClient nodes
- 1 Go-Ethereum private net node (simulating the GOERLI testnet with a chain ID of 1337)
- 1 Bitcoin core private node (still in the planning phase and not implemented yet)
- 1 Orchestrator node that oversees the coordination of the smoke tests.
- relevant [code path](https://github.com/zeta-chain/node/tree/develop/contrib/localnet/orchestrator/smoketest) and location of the testing

![Screenshot 2023-10-30 at 9.30.57 AM.png](Testing%20101%20+%20Best%20Resources%20b7a87329bd7242bd8952e310cccce0ab/Screenshot_2023-10-30_at_9.30.57_AM.png)

**Running the Integration Tests**:

- Build the ZetaNode with the command:

```bash
make zetanode
```

- Following that, execute the subsequent test command

```bash
make start-smoketest
```

- To view the logs, we can use the command to dump the log into the file “output.log” and examine the integration testing:

```bash
docker logs orchestrator > output.log
```

![Screenshot 2023-10-29 at 5.28.32 PM.png](Testing%20101%20+%20Best%20Resources%20b7a87329bd7242bd8952e310cccce0ab/Screenshot_2023-10-29_at_5.28.32_PM.png)

after we run the test, we can monitor the test progress as well

```bash
make start-smoketest
```

![Screenshot 2023-10-29 at 5.20.35 PM.png](Testing%20101%20+%20Best%20Resources%20b7a87329bd7242bd8952e310cccce0ab/Screenshot_2023-10-29_at_5.20.35_PM.png)

we can use the docker ps command to view the running docker container

![Screenshot 2023-10-29 at 5.21.42 PM.png](Testing%20101%20+%20Best%20Resources%20b7a87329bd7242bd8952e310cccce0ab/Screenshot_2023-10-29_at_5.21.42_PM.png)

or we can use the docker desktop client to view the container

![Screenshot 2023-10-29 at 5.23.31 PM.png](Testing%20101%20+%20Best%20Resources%20b7a87329bd7242bd8952e310cccce0ab/Screenshot_2023-10-29_at_5.23.31_PM.png)

then we can monitor the testing by view the log of the orchestrator container

```bash
docker logs -f orchestrator
```

Security reseachers can investigate the test cases that are failing during integration, as these errors can provide insights into the location and nature of potential bugs

## **Example** ************Testing Strategy and Test Case:************

 While there are existing unit and integration tests, more exotic edge cases beyond the obvious and common execution paths have not been covered.  The following list are left to the reader to implement as possible avenues for uncovering attack vectors. 

### **Inbound Transaction Observation Test Cases:**

1. **Basic Flow**:
    - Positive Test: Send a CCTX to ZetaChain with a valid message. Ensure that ZRC20 tokens are correctly deposited and the contract on ZetaChain is triggered.
    - Negative Test: Send a CCTX to ZetaChain without any message. Confirm that no ZRC20 tokens are deposited and no contract on ZetaChain is triggered.
2. **Contract Address and Arguments Validity**:
    - Positive Test: Send a CCTX with a valid contract address and arguments. Check for successful execution.
    - Negative Test: Send a CCTX with an invalid contract address but valid arguments. Ensure the transaction fails.
3. **Chain Destination Handling**:
    - Positive Test: Send a CCTX where the destination chain is not ZetaChain. Confirm that the status changes to "pending outbound".
    - Negative Test: Send a CCTX without specifying a destination chain. Ensure that there's a clear error or default behavior.
4. **Token Amount Verification**:
    - Positive Test: Send a CCTX with a specified amount of ZRC20 tokens. Ensure that the exact amount is minted/burned on ZetaChain.
    - Negative Test: Send a CCTX with a zero or negative token amount. Confirm that the transaction is either rejected or handled appropriately.
5. **Gas Payment Handling**:
    - Positive Test: Ensure that when ZRC20 tokens are deposited, the appropriate amount of gas is charged.
    - Negative Test: Send a CCTX with insufficient gas. Ensure that the transaction is rejected.
6. **Concurrency and Multiple Transactions**:
    - Positive Test: Send multiple valid CCTXs in rapid succession. Verify that all transactions are processed correctly without any loss or mismanagement of tokens.
7. **Rate Limiting and Throttling**:
    - Negative Test: Send a large number of CCTXs in a very short time span to test the system's rate limiting and throttling capabilities.
8. **Replay Attack Prevention**:
    - Negative Test: Send a CCTX, then try to send the same CCTX again. Ensure that the second transaction is rejected or handled appropriately to prevent double deposits.
9. **Transaction Timeout**:
    - Negative Test: Initiate an inbound transaction but let it not complete. Check how the system handles transactions that don't complete within a given timeframe.
10. **Network Issues and Recovery**:
    - Negative Test: Simulate network disconnections or delays during the CCTX process. Ensure that transactions can recover once the network is stable, or they fail gracefully.
11. **Error Messaging**:
    - Negative Test: Send various invalid CCTXs (e.g., wrong format, missing fields, incorrect token amounts). For each, verify that the error messages returned are descriptive and helpful.
12. **Transaction Logging and Monitoring**:
    - Positive Test: For every inbound transaction, ensure there is a detailed log available which can be used for troubleshooting or audit purposes.
13. **Fees and Additional Costs**:
    - Positive Test: Send a CCTX and confirm that any additional fees or costs associated with the transaction (beyond the gas) are correctly applied and transparent to the user.
14. **External Chain Behavior**:
    - Negative Test: Send a CCTX from an external chain that is experiencing congestion or issues. Observe how ZetaChain handles these potentially delayed or problematic transactions.
15. **Third-party Service Integration**:
    - Positive Test: If there are any third-party services integrated (like oracles), ensure their data input or triggers work seamlessly with the inbound transaction process.

**Outbound Transaction Observation Test Cases**:

1. **Basic Flow**:
    - **Positive Test**: Initiate an outbound transaction from ZetaChain. Ensure observers correctly watch the transaction and status changes from "pending outbound" to "final" upon confirmation.
    - **Negative Test**: Initiate an outbound transaction without proper configuration or missing fields. Confirm that the transaction is rejected or handled appropriately.
2. **Observer Monitoring**:
    - **Positive Test**: Ensure observers are actively monitoring ZetaChain for pending outbound transactions and correctly entering the TSS keysign ceremony.
    - **Negative Test**: Disable or reduce observer count and initiate an outbound transaction. Check for system resilience and error handling.
3. **TSS Keysign Ceremony**:
    - **Positive Test**: Ensure that when an observer enters into a TSS keysign ceremony, the transaction is correctly signed.
    - **Negative Test**: Tamper with the transaction data during the keysign ceremony. Confirm that the compromised transaction is rejected.
4. **Transaction Broadcasting to Connected Blockchains**:
    - **Positive Test**: After the keysign ceremony, ensure the signed transaction is correctly broadcasted to the connected blockchains.
    - **Negative Test**: Simulate a broadcast failure. Check for system resilience and recovery mechanisms.
5. **Observing Outbound Transaction Confirmation**:
    - **Positive Test**: Ensure that observers correctly monitor the connected blockchains for transaction confirmations.
    - **Negative Test**: Delay or prevent a transaction from being confirmed on the connected blockchain. Observe how the ZetaChain system reacts to such delays.
6. **Voting on Observed Outbound Transaction**:
    - **Positive Test**: After a transaction is confirmed on a connected blockchain, ensure observers vote on ZetaChain and the voting process functions smoothly.
    - **Negative Test**: Simulate a scenario where not enough votes are acquired. Verify how the system handles such scenarios.
7. **Gas Payment on Outbound Transactions**:
    - **Positive Test**: Ensure that the correct amount of gas is deducted for each type of CCTX (Gas cctx, Zeta cctx, ERC20 cctx) and the swaps function correctly.
    - **Negative Test**: Send an outbound transaction without sufficient gas or using a non-supported CCTX type. Verify rejection or appropriate handling.
8. **Gas Price Increase Mechanism**:
    - **Positive Test**: For an outbound transaction that's pending for an extended period, ensure that the gas price increases as specified.
    - **Negative Test**: If there are insufficient funds in the gas stability pool, ensure that the gas price does not increase.
9. **Concurrency and Multiple Transactions**:
    - **Positive Test**: Initiate multiple outbound transactions in rapid succession. Confirm that all are processed correctly without any issues.
10. **Replay Attack Prevention**:
    - **Negative Test**: Send an outbound transaction, then try to resend the same transaction. Confirm the second transaction is handled appropriately to prevent double processing.
11. **Error Messaging and Notifications**:
    - **Negative Test**: Initiate outbound transactions with various issues (e.g., wrong format, missing fields). Verify that error messages are descriptive and helpful.
12. **Rate Limiting and Throttling**:
    - **Negative Test**: Send a very large number of outbound transactions in a short time span. Observe the system's rate limiting and throttling response.
13. **Network Issues and Recovery**:
    - **Negative Test**: Simulate network disconnections or delays during the outbound transaction process. Verify that the system can recover or handle network instabilities gracefully.
14. **External Chain Behavior**:
    - **Negative Test**: Send an outbound transaction to an external chain experiencing congestion or issues. Confirm ZetaChain's response to problematic external chains.
15. **Third-party Service Integration**:
    - **Positive Test**: If third-party services (e.g., oracles or external APIs) are integrated, ensure their inputs or triggers work well with the outbound transaction process.
16. **Monitoring and Logging**:
    - **Positive Test**: Ensure detailed logs are available for every step of the outbound transaction process for troubleshooting or audit purposes.

**Permissionless Tx Validation Model**

1. **Block Header Addition and Storage**:
    - Validate that block headers from Ethereum/BSC and Bitcoin are correctly added and stored on ZetaChain.
2. **Inbound/Outbound TX Proof Validation**:
    - Check the system to ensure that inbound and outbound tx proofs validate correctly against stored block headers.
    - Negative Test: Ensure that incorrect proofs are rejected.
3. **Tx Tracker**:
    - Validate that transactions are correctly added to both inTxTracker and outTxTracker.
    - Confirm that these transactions validate correctly against the provided proofs and stored headers.

**Permissions and Admin Groups**

1. **Action Permissions**:
    - Check that only the specified admin group can execute each action.
    - Negative Test: Attempt to execute an action as a non-admin or the wrong admin group.
2. **Emergency and Operational Actions**:
    - Confirm that emergency actions by Group1 execute rapidly and halt the workflow.
    - Test that operational actions by Group2, like updating the system contract, require multiple signatures and function as expected.
3. **Permissions Table**:
    - Walkthrough each message in the permissions table, ensuring they can only be executed by the correct admin group and that they perform their specified action correctly.

**Gas Payment on ZetaChain for Outbound TX**:

- Test each type of cctx (Gas cctx, Zeta cctx, ERC20 cctx) to ensure gas is deducted and swapped correctly.
- Verify that the Uniswap pools on ZetaChain are being used correctly and that liquidity is maintained.

**Gas Price Increase for Outbound TX**:

- Check the mechanism that increases the gas price if an outbound tx isn't mined quickly.
- Ensure that the surplus gasUsed/gasLimit is correctly sent to the "gas stability pool."
- Confirm that if a transaction is pending for too long, the gas price increases and that the funds from the gas stability pool cover this increase.
- Check that if there aren't enough funds in the gas stability pool, the gas doesn't increase.

### Additional Considerations:

1. **Load Testing**: Considering the complexity of the protocol, it's essential to perform load testing to ensure the system can handle a high volume of cross-chain transactions.
2. **Security Testing**: Given the financial nature of blockchain protocols, penetration testing should be carried out to identify potential vulnerabilities.
3. **Edge Cases**: Given the intricacies and multiple functionalities, it's crucial to test edge cases like very low or very high amounts, rapid repeated requests, and random data input.
4. **Integration Testing**: As the system interacts with external chains, integration testing is crucial to ensure smooth interoperability.
5. **Rollback Testing**: In blockchain systems, it's essential to see how the system behaves after a rollback, especially if an erroneous transaction gets into the blockchain.
6. **Cross-chain Testing**: Ensure tests cover both EVM-to-EVM calls and Bitcoin network (non-EVM)-to-EVM command calls.

### **Potential Attack Vectors**

- Error handling and panic handling - craft payload to crash the validator
- Use vulnerable version of library
- Overflow / underflow
- Transaction replay and double spending
- Misconfiguration
- Access control

### **Resources:**

- [https://go.dev/doc/tutorial/add-a-test](https://go.dev/doc/tutorial/add-a-test)
- [https://blog.jetbrains.com/go/2022/11/22/comprehensive-guide-to-testing-in-go/](https://blog.jetbrains.com/go/2022/11/22/comprehensive-guide-to-testing-in-go/)
- [https://tutorials.cosmos.network/hands-on-exercise/2-ignite-cli-adv/7-integration-tests.html](https://tutorials.cosmos.network/hands-on-exercise/2-ignite-cli-adv/7-integration-tests.html)
- [https://tutorials.cosmos.network/academy/2-cosmos-concepts/12-testing.html#testing-pyramid](https://tutorials.cosmos.network/academy/2-cosmos-concepts/12-testing.html#testing-pyramid)
- [https://deft-duckanoo-3b3f15.netlify.app/#file273](https://deft-duckanoo-3b3f15.netlify.app/#file273) (unit test of the validator coverage report)
- [https://www.halborn.com/blog/post/top-5-security-vulnerabilities-cosmos-developers-need-to-watch-out-for](https://www.halborn.com/blog/post/top-5-security-vulnerabilities-cosmos-developers-need-to-watch-out-for)
- [https://medium.com/coinmonks/a-glance-at-cosmos-security-fdb0b0ad6e7](https://medium.com/coinmonks/a-glance-at-cosmos-security-fdb0b0ad6e74)
- [https://github.com/zeta-chain/node/blob/develop/Makefile](https://github.com/zeta-chain/node/blob/develop/Makefile)
- [https://github.com/zeta-chain/node/blob/develop/contrib/localnet/README.md](https://github.com/zeta-chain/node/blob/develop/contrib/localnet/README.md)
