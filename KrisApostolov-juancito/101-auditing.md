# Auditing 101

## Auditing ZetaChain

### Blockchain Vulnerabilities

ZetaChain is a Layer 1 blockchain, and as such, it may be subject to blockchain-specific impacts, different than smart contracts ones.

Some examples are:

- Network not being able to confirm new transactions
- Unintended permanent chain split requiring hard fork
- Cryptographic flaws that could result in leaking private key or allow forging signatures
- Tamper / manipulate blockchain history to add, invalidate, or change past transactions
- Temporary Consensus Failures
- Cheap Denial of Service of Critical Functions of the blockchain

For a broader list of impacts, the [Severity Classification Impact Definitions by Immunefi](https://immunefisupport.zendesk.com/hc/en-us/articles/13333032674961-Severity-Classification-System#h_01H9R4VPMVGM18EWR7JW3EZXQW) has more examples.

The [ZetaChain Bug Bounty Program](https://immunefi.com/bounty/zetachain/) has a list of impacts that are of particular interest and specific to the chain.

In addition, you can check [Blockchain Common Vulnerability List](https://github.com/slowmist/Cryptocurrency-Security-Audit-Guide/blob/main/Blockchain-Common-Vulnerability-List.md), and [Blockchain-Attack-Vectors](https://github.com/demining/Blockchain-Attack-Vectors) for more ideas on blockchain-specific attacks.

### Cosmos SDK Security

ZetaChain is based on Cosmos SDK, which introduces a set of specific vulnerabilities like for example:

- Incorrect signers - Broken access controls due to incorrect signers validation
- Not prioritized messages - Risks arising from usage of not prioritized message types
- Panics and unbounded computation in BeginBlock/EndBlock which can result in denial of service.

If you want to learn more about Cosmos SDK vulnerabilities:

- [Exploring Cosmos: A Security Primer](https://www.zellic.io/blog/exploring-cosmos-a-security-primer) by Zellic is a good starting point to understand risks associated to the Cosmos SDK.
- Trail of Bits maintains a repository with common [Cosmos applications vulnerabilities](https://github.com/crytic/building-secure-contracts/tree/master/not-so-smart-contracts/cosmos).
- Halborn has an interesting article called [Top 5 Security Vulnerabilities Cosmos Developers Need to Watch out for](https://www.halborn.com/blog/post/top-5-security-vulnerabilities-cosmos-developers-need-to-watch-out-for).
- [The Challenge of Go’s Map Iteration in the Cosmos SDK Blockchain: A Dive into Determinism](https://ashourics.medium.com/the-challenge-of-gos-map-iteration-in-the-cosmos-sdk-blockchain-a-dive-into-determinism-bd5a99260519) by Mo Ashouri is another good read on this specific issue.

### Go Bugs

ZetaChain [node](https://github.com/zeta-chain/node) is written in Go, and might be contain language-specific bugs as:

- Integer overflows
- Non-determinism
- Loss in precision / Issues with Float associativity

Here are some tools that can help in the detection of such bugs:

- [staticcheck](https://staticcheck.dev/) - It finds bugs and performance issues, offers simplifications, and enforces style rules.
- [gosec](https://github.com/securego/gosec) - Inspects source code for security problems by scanning the Go AST and SSA code representation.
- [unconvert](https://github.com/mdempsky/unconvert) - Remove unnecessary type conversions from Go source.
- [codeql](https://codeql.github.com/) - Semantic code analysis engine to discover, and write queries to find all variants of a vulnerability.
- [ineffassign](https://github.com/gordonklaus/ineffassign) - Detect ineffectual assignments in Go code.
- [semgrep](https://github.com/returntocorp/semgrep) - Static analysis tool for finding bugs, detecting vulnerabilities in third-party dependencies, and enforcing code standards.

## Creating a POC

You can test the node by following [ZetaChain Local Net Development & Testing Environment](https://github.com/zeta-chain/node/blob/develop/contrib/localnet/README.md). It includes a suite of smoke tests covering many aspects of the codebase.

There's currently no functionality to debug specific features, but you can reduce the smoke tests run by commenting the optional ones in the main file to only run the test of interest.

This way you can create a POC and test the specific files you want, saving time, and without the need to run the whole test-suite.

You can do it by modifying the [smoketest/main.go](https://github.com/zeta-chain/node/blob/develop/contrib/localnet/orchestrator/smoketest/main.go#L265) file.

## Additional Resources

- [ZetaChain Previous Audits](https://drive.google.com/drive/folders/10PFcoASYKhllalv5n1AW4mYD12urPgWJ)
- [Static Application Security Testing of Consensus-Critical Code in the Cosmos Network](https://www.researchgate.net/publication/373262833_Static_Application_Security_Testing_of_Consensus-Critical_Code_in_the_Cosmos_Network)
- [Crosschain Risk Framework](https://crosschainriskframework.github.io/)
- [Awesome Cosmos](https://github.com/cosmos/awesome-cosmos#tools)
