# Project 101

## Table of Contents

  * [Introduction](#introduction)
    + [Resources](#resources)
    + [Cosmos Ecosystem](#cosmos-ecosystem)
    + [Cosmos SDK, Tendermint Consensus and IBC](#cosmos-sdk--tendermint-consensus-and-ibc)
    + [Cosmos Infrastructure Related Vulnerabilities](#cosmos-infrastructure-related-vulnerabilities)
  * [ZetaChain Architecture](#zetachain-architecture)
    + [ZetaChain Blockchain Sovereignty / Interchain Security](#zetachain-blockchain-sovereignty---interchain-security)
    + [Blockchain Interoperability](#blockchain-interoperability)
    + [Validators, Observers and Signers](#validators--observers-and-signers)
    + [Threshold Signature Scheme - Security Considerations](#threshold-signature-scheme---security-considerations)
    + [ThorChain, SifChain and ChainFlip](#thorchain--sifchain-and-chainflip)
- [Development Considerations for zeta-chain/node](#development-considerations-for-zeta-chain-node)
  * [Setting up the development environment](#setting-up-the-development-environment)
  * [ZetaChain Architecture](#zetachain-architecture-1)
    + [Key Architecture Patterns](#key-architecture-patterns)
    + [Modules](#modules)
    + [Keepers](#keepers)
    + [Stores and Object Capabilities](#stores-and-object-capabilities)
    + [CometBFT](#cometbft)
    + [ABCI](#abci)
    + [Protobuf](#protobuf)
    + [Messages](#messages)
    + [Queries](#queries)
  * [Essential Folder Breakdown](#essential-folder-breakdown)
  * [Resources for the Go language](#resources-for-the-go-language)

![Untitled](Project%20101%2044d769e6aae4459d85b91ddbba34ae81/Untitled.png)

## Introduction

This Project 101 primer will give an already professional and competent security researcher a guide to the ZetaChain platform, how it functions and similarities/differences to other platforms in its class. The primer will streamline on-boarding of security researchers new to or inexperienced with this technology stack. We achieve this objective by analysing core underlying Cosmos Infrastructure used to build ZetaChain and providing a robust overview of ZetaChain’s current architecture. Furthermore, we deep dive into developer considerations for the zeta-chain/node and outline key resources to accelerate Go learning.

### Resources

The first resource that can and should be utlized to understand ZetaChain is the protocols whitepaper ([https://www.zetachain.com/whitepaper.pdf](https://www.zetachain.com/whitepaper.pdf)) , in which the project team describes the blockchain as implementing “generic omnichain smart contract support that connects both smart contract blockchains such as Ethereum..and even non smart contract blockchains such as Bitcoin and Dogecoin”. As such, it can be observed that at the core of ZetaChain ‘ideology’ and use-case is the enablement of interoperability between blockchains. Although the ZetaChain project is built within the Cosmos Ecosystem, which has become renowned  for its standardized methods for ‘out of the box’ interoperability between chains, ZetaChain is a sovereign blockchain and implements its own methods for achieving multi/omni-chain functionality.

Further resources used in composing this Alpha article, including independent security research, project specific documentation and exploit write-ups will be referenced for readers to explore at their choosing.

### Cosmos Ecosystem

Furthermore, a core concept that must be first understood by security researchers when seeking to understand the ZetaChain blockchain, is that it is built within the Cosmos ecosystem. The Cosmos ecosystem has commonly become to be known as “a network of blockchains” ( [https://tokeninsight.com/en/tokenwiki/all/what-is-cosmos](https://tokeninsight.com/en/tokenwiki/all/what-is-cosmos)) due to underlying projects being implemented as independent, proof-of-stake, layer one blockchains that have their own validators and produce their own blocks. Within the Cosmos documentation these projects are generally referred to as “application-specific blockchains”.

Projects built in the Cosmos ecosystem have specific architectural properties and requirements, creating both operational effectiveness and potential security weaknesses that should be considered in both development and review processes. In Project 101 we will focus on the functionality of ZetaChain, in the context of it being built in the Cosmos ecosystem. This is still with a view that the audience are primarily security researchers, noting security considerations where relevant. However, please bear in mind that Testing 101 will specifically address explicit security concerns, the specific security model deployed and what tools could be used to help test or analyze the codebase.

Cosmos ecosystem projects, including ZetaChain are constructed using components such as the Cosmos Software Development Kit (SDK), Tendermint Consensus Engine and the Inter-Blockchain Communication Protocol (IBC). We will describe and analyze each of these key architectural components individually, to help security researchers new to the technology understand areas of potential vulnerability.

### ************************************************************Cosmos SDK, Tendermint Consensus and IBC************************************************************

Cosmos SDK is a “framework for building blockchain applications” ([https://pkg.go.dev/github.com/cosmos/cosmos-sdk](https://pkg.go.dev/github.com/cosmos/cosmos-sdk)) built in the Go language. The SDK is used to build build blockchains from ‘composable modules’, these modules can be seen as state-machines within the state machine. It should be noted that developers can pick and choose ‘out-of-the-box’ modules provided by the Cosmos SDK dependant on its needs. The modules that ZetaChain has utilized ‘out-of-the-box’ from the Cosmos SDK are listed below.

- `auth`
- `bank`
- `staking`
- `slashing`
- `evidence`
- `gov`
- `distribution`
- `upgrade`
- `feemarket`
- `authz`
- `group`
- `crisis`
- `params`

Further detailed information on modules and their role within the Cosmos SDK can be found at ([https://docs.cosmos.network/main/build/building-modules/intro](https://docs.cosmos.network/main/build/building-modules/intro)).

Conversely, custom modules will allow developers to implement the specific business logic of their application. Within the ZetaChain project there are a number of specific custom modules that are utilized to implement the ZetaChain project’s business logic. These modules will be a specific area of scrutiny that security researchers will want to deep dive and robustly test developer assumptions and attack vectors. We list the custom modules that ZetaChain have implemented below.

- [crosschain](https://www.zetachain.com/docs/architecture/modules/crosschain/overview/):  tracks the state of cross-chain transactions
- [emissions](https://www.zetachain.com/docs/architecture/modules/emissions/overview/): orchestrates the distribution of rewards for observers, validators, and TSS signers
- [fungible](https://www.zetachain.com/docs/architecture/modules/fungible/overview/): provides mechanisms for deploying fungible tokens from other chains
- [observer](https://www.zetachain.com/docs/architecture/modules/observer/overview/): tracks voting, observer parameters and administration, and validates cross-chain data from the `crosschain` module

The Inter-Blockchain Communication (IBC) protocol can be described as a TCP/IP-like protocol for communication between sovereign blockchains. IBC is an end-to-end, connection oriented, stateful protocol between blockchains. IBC provides chains with a common protocol and framework for implementing standardized inter-blockchain communication. Comprehensive information regarding IBC and methods of implementation can be found at the official Cosmos Documentation repository ([https://tutorials.cosmos.network/academy/3-ibc/1-what-is-ibc.html](https://tutorials.cosmos.network/academy/3-ibc/1-what-is-ibc.html)). However, it must be noted that IBC is not the primary method that ZetaChain achieves its use-case of blockchain interoperability. ZetaChain specific architecture and cross-chain communication methods will be outlined further below. 

Tendermint Consensus Engine ********is, as the name suggests, the means by which the blockchain reaches consensus, ensuring that the same transactions are recorded on every machine in the same order. It also contains what is known as an Application Blockchain Interface (ABCI) which is the interface between the consensus engine and application process. It should be noted that the nomenclature around the Tendermint Consensus Engine can be somewhat confusing, in the ZetaChain Official Documentation ([https://www.zetachain.com/docs/architecture/overview/](https://www.zetachain.com/docs/architecture/overview/)) it is referred to as Tendermint PBFT (Practical Byzantine Fault Tolerant) and in the official Cosmos Educational Documentation ([https://tutorials.cosmos.network/academy/2-cosmos-concepts/1-architecture.html#consensus-in-tendermint-core-and-cosmos](https://tutorials.cosmos.network/academy/2-cosmos-concepts/1-architecture.html#consensus-in-tendermint-core-and-cosmos)) we are introduced to the consensus engine and generic application interface as CometBFT.

After clarification with the ZetaChain team, researchers should note that ZetaChain is using Cosmos SDK v46.13 which is using CometBFT for the consensus engine. It should be noted that that CometBFT is a fork of Tendermint Core that is now the actively maintained Consensus Engine for Cosmos (Tendermint Core is no longer active). This information should help researchers cut through different naming conventions in different documentation resources available and clarify the components currently in use.

### ******************************************Cosmos Infrastructure Related Vulnerabilities******************************************

It is important for us to understand what specific components from the Cosmos stack are used in building ZetaChain, as Cosmos Infrastructure related vulnerabilities may be a key source of compromise for ZetaChain and indeed other Cosmos Ecosystem projects.

A seminal example of a Cosmos Infrastructure related vulnerability was written up by Maxwell Dulin ([https://maxwelldulin.com/BlogPost/stdout-cosmos-sdk-rce](https://maxwelldulin.com/BlogPost/stdout-cosmos-sdk-rce)), an incredibly talented security researcher. In this write up, the vulnerability relates to `cosmovisor` which is described as a “small process manager for Cosmos SDK application binaries that monitors **`stdout`** for incoming chain upgrade proposals. If it sees a proposal that gets approved, cosmovisor can automatically download the new binary, ..., switch from the old binary to the new one, and finally restart the node with the new binary.” ([https://github.com/cosmos/cosmos-sdk/tree/cosmovisor/v0.1.0/cosmovisor#cosmosvisor-quick-start](https://github.com/cosmos/cosmos-sdk/tree/cosmovisor/v0.1.0/cosmovisor#cosmosvisor-quick-start))

Maxwell observed that all that would be needed to successfully exploit a protocol using `cosmovisor`, would be to write to `stdout` and the process manager would update your binary. Although we will not review the deeper techincal details of the vulnerability or methodology for the exploit, Maxwell does successfully demonstrate that a underlying issue in Cosmos Infrastructure can result in Remote Code Exploitation (RCE) or Denial of Service (DoS) scenarios. This research demonstrates that projects using Cosmos Infrastructure must carefully review their implementations and integrations with the stack.

Although ZetaChain is not vulnerable to this specific bug due to using Cosmos SDK v46.13, it is also a clear demonstration that security researchers need to be aware of the security implications of using Cosmos Infrastructure components. These implications should also influence a security researchers potential project plan when conducting his/her security review of ZetaChain and other Cosmos Ecosystem projects.

Furthermore, we can also look at ways in which non-determinism can be caused in the network as a result of infrastructure related vulnerabilties. The infamous Juno exploit ([https://blockpane.medium.com/who-crashed-junø-3cd889b3524](https://blockpane.medium.com/who-crashed-jun%C3%B8-3cd889b3524)) in which a CosmWasm bug allowed making consensus breaking queries, led to the halt of the Juno chain. Although ZetaChain does not use CosmWasm, security researchers should still be cogniscant of vulnerabilites that lie within infrastructure that could lead to non-determinism in the the network,. Halborn Security gave an in depth breakdown of this topic ([https://www.halborn.com/blog/post/top-5-security-vulnerabilities-cosmos-developers-need-to-watch-out-for](https://www.halborn.com/blog/post/top-5-security-vulnerabilities-cosmos-developers-need-to-watch-out-for)) within an article they published in December 2022. 

There are other specific vulnerabilties that can arise from use of the Go programming language itself. In another article published by Halborn Security ([https://www.halborn.com/blog/post/dont-panic-how-improper-error-handling-can-lead-to-blockchain-hacks](https://www.halborn.com/blog/post/dont-panic-how-improper-error-handling-can-lead-to-blockchain-hacks)) , the author describes how use of Panics for Error Handling can lead to an attacker using the discovering a crash and then escalating into a full exploit.  Panics are a construction used in Go and Rust to handle errors. Now while this is not strictly speaking ‘Cosmos Infrastructure’ it is a consideration that previous Solidity/EVM focused security researchers would not usually consider. As such, we recommend security researchers new to ZetaChain and Cosmos explore this topic in depth so they can better understand the security implications associated with Go usage.

## Z**etaChain Architecture**

### **ZetaChain Blockchain Sovereignty / Interchain Security**

As aforementioned, ZetaChain is a sovereign blockchain within the Cosmos Ecosystem. This means that it has its own nodes, validators and produces its own blocks. As also previously discussed, security researchers should not be confused with the interoperability aspect of the Cosmos stack using IBC, as ZetaChain uses its own bespoke architecture to achieve this use-case.

Another aspect to ensure that security researchers are aware of is that ZetaChain does not utilize ‘Interchain Security’. Interchain Security is one of the propositions that Cosmos markets to developers to secure protocols that reside in their ecosystem.  We will not deep dive into the technical implementation of Interchain Security but at a high level it allows sovereign blockchains or ‘App-Chains’ as they are known in the Cosmos universe to rent the Cosmos Hub’s security assurances ([https://medium.com/coinmonks/a-glance-at-cosmos-security-fdb0b0ad6e74](https://medium.com/coinmonks/a-glance-at-cosmos-security-fdb0b0ad6e74)) . This is done through allowing validators on a provider chain to use stake on that chain to concurrently secure a consumer chain. In exchange for protecting the consumer chain, the provider chain is compensated with a cut of the consumer chain's gas costs and block rewards.

For clarity, ZetaChain will not use Interchain Security. After clarification with the ZetaChain team, it is confirmed that ZetaChain will be a decentralized proof of stake from day 1 at mainnet and not use to Interchain Security proposition offered by Cosmos. Furthermore, we obtained valuable information that the maximum number of validators will be 125. In addition, for the specific interoperability components will have a permissioned subset of 9 Observer validators initially. 

The concept of Observer and Signer validators within the ZetaChain architecture will be explored further below. However, the level of decentralization in regards to the validator landscape is an important consideration that researchers should be aware of when assessing ZetaChain’s security model. We will expand upon this in the next section.

### Blockchain Interoperability

To achieve blockchain interoperability ZetaChain uses a “decentralized notary scheme on top of an incentivized Proof-of-Stake replicated state machine” ([https://www.zetachain.com/whitepaper.pdf](https://www.zetachain.com/whitepaper.pdf)). We critically analyze these components together with similarities and differences to projects in the same ‘class’ of protocol that enable blockchain interoperability below.

ZetaChain’s Whitepaper includes a robust survey of current cross-chain strategies including Relays, Notary Schemes, Hash Time-Lock Contracts and Blockchain of Blockchains. We recommend security researchers to deep dive into these strategies to better understand the inherent security implications of different models. Understanding which strategies are used for blockchain interoperability will again influeunce a security researchers scope and methodology for review.

The first element of ZetaChain’s specific architecture for cross-chain interoperability is the Notary Scheme. A Notary Scheme is where “a trusted entity (or a set of) is tasked with notarizing claims such as event X has happened on blockchain A”. In ZetaChain’s architecture these trusted entities consist of Observers watching for relevant transactions on external blockchains and Signers holding a key share that are responsible for changing state on the blockchain where relevant.

### Validators, Observers and Signers

As stated in ZetaChain’s official documentation, the protocols architecture consists of a distributed network of nodes, hereafter reffered to as Validators. These Validators can be grouped into three main categories. We paraphrase the official ZetaChain definitions below and add critical analysis thereafter.

Basic Validators: Each validator node can vote on block proposals with voting power proportional to the staking coins (ZETA) bonded/delegated. Each validator is identified by its consensus public key. Validators need to be online all the time, ready to participate in the constantly growing block production. In exchange for their service, validators will receive block rewards and transaction fees.

Critical Analysis: As aforementioned, we draw readers attention to the difficulty for ZetaChain and indeed other Cosmos App-Chains to bootstrap “an initial collection of validators” ([https://medium.com/coinmonks/a-glance-at-cosmos-security-fdb0b0ad6e74](https://medium.com/coinmonks/a-glance-at-cosmos-security-fdb0b0ad6e74)). ZetaChain describes their security model as reliant on decentralization (Whitepaper 7.1). However, security researchers must be cognizant that in the initial stages of production Validators may be heavily centralized leading to potential abuse and malicious behavior.

Observers: The observers watch externally connected chains for certain relevant transactions/events/states at particular addresses via their full nodes of external chains. Observers will divide into two roles: sequencer and verifier. The sequencer discovers relevant external trans-actions/events/states and reports to verifiers; the verifiers verify and vote on ZetaChain to reach consensus. The system requires at least one sequencer and multiple verifiers. The sequencer does not need to be trusted, but at least one honest sequencer is needed for liveness.

Critical Analysis: With Observers forming a key architectural component of how ZetaChain acheives blockchain interoperability, it is crucial that Observers are robustly tested to check developer assumptions are correct and state cannot be changed maliciously. 

TSS Signers: ZetaChain collectively holds standard ECDSA/EdDSA keys for authenticated interaction with external chains. The keys are distributed among multiple signers in such a way that only a super majority of them can sign on behalf of the ZetaChain. It's important to ensure that at no time is any single entity or small fraction of nodes able to sign messages on behalf of ZetaChain on external chains. The ZetaChain system uses bonded stakes and positive/negative incentives to ensure economic safety.

Critical Analysis: Potential centralization of validators could lead to compromise of the private key. We will analyze specific security considerations when using the Threshold Signature Scheme (TSS) in the next section

### Threshold Signature Scheme - Security Considerations

It is an important architectural decision that collectively all validators hold a single public/private key pair which can initiate transactions on other blockchains to change state directly. ZetaChain utilizes the Multi-Party Threshold Signature Scheme (TSS) 

However, security researchers must note that the security model relies on the assumption that a super majority (>66% nodes) are acting in a honest manner and do not collude. If they were to collude it would be possible for malicious transactions to be signed. We recommend for security researchers to deep dive into the current validator landscape and examine if it is possible to exploit the ZetaChain’s current validator landscape.

Furthermore, we draw security researchers to incredibly relevant research conducted by Verichains ([https://www.verichains.io/tsshock/#intro](https://www.verichains.io/tsshock/#intro)) regarding compromise and exploitation of Threshold Signature Scheme (TSS). Verichains discovered that several popular implementations in Go and Rust were vulnerable to key extraction attacks. We recommend security researchers review Verichains ‘TSSHOCK’ exploit and determine if it can be applied to ZetaChain’s architecture.

### ThorChain, SifChain and ChainFlip

The ZetaChain team explicity state that ThorChain is one of the key sources of inspiration from an architectural perspective for their project. As such, it is prudent to analyze any architectural concerns, exploits or security vulnerabilties applicable to ThorChain and crtically analyze if they could also be applicable to ZetaChain.

SifChain and ChainFlip also use a similar architecture to ThorChain. As such, we also recommend that security issues that have been identified in relation to these protocols and examined if applicable or relevant to ZetaChain.

Potential comparisons can alos be drawn between Juno (an open-source platform for interoperable smart contracts) and Evmos (”the first decentralized EVM chain on the Cosmos Network”).

# Development Considerations for zeta-chain/node

This section is written to achieve the following outcomes:

1. You will have [cloned, compiled, and smoke-tested](https://www.notion.so/Project-101-44d769e6aae4459d85b91ddbba34ae81?pvs=21) the ZetaChain node (`zetacored`) to confirm the development environment is installed correctly
2. You will have [understood the node’s code structure the descriptions of key elements](https://www.notion.so/Project-101-44d769e6aae4459d85b91ddbba34ae81?pvs=21) of the architecture

<aside>
❗ **How to use this section:**

*Expect important terms and concepts to link to relevant documentation!* 

While the main points shared here are contained to a 10,000ft view to get you up to speed, make sure to drill deeper by clicking the linked terms as necessary.

</aside>

## Setting up the development environment

1.  Install Go
    - Use version 1.20.10 [https://go.dev/dl/#go1.20.10](https://go.dev/dl/#go1.20.10). Use `go version` to check the version you have installed.
2. [Install Docker](https://docs.docker.com/engine/install/)
    - Make sure docker version is ≥ 23 to so [BuildKit](https://docs.docker.com/build/buildkit/) is enabled
        - Otherwise, you can [enable it separately](https://docs.docker.com/build/buildkit/#getting-started:~:text=To%20set%20the,command%2C%20run)
        - If you’re having an issue trying to [set up the package repository](https://www.notion.so/f7c0f29d18194e2faf01ed2c0e60ddf4?pvs=21) on `apt` -based Linux distributions (like Ubuntu) and you can’t get the “keyring” to update, make sure the directory “/etc/apt/keyrings” exists
3. [Clone the repository and build the testnet](https://github.com/zeta-chain/node#building-the-zetacoredzetaclientd-binaries)
    - Clone the repository at [https://github.com/zeta-chain/node](https://github.com/zeta-chain/node)
    - In the cloned repo, build the testnet with the command:
    
    ```bash
    make install-testnet
    ```
    
    - You likely will need to [ensure your PATH includes](https://opensource.com/article/17/6/set-path-linux) `~/go/bin` for the subsequent commands to work
4. Check that the install worked by running these commands:
    
    ```bash
    zetacored version
    zetaclientd version
    ```
    
5. [Run the Smoke Test to get the environment up and running by running these commands in the clone repo’s directory](https://github.com/zeta-chain/node/blob/develop/contrib/localnet/README.md)
6. Check that the Smoke Test has run completely and successfully
    - This can take a few minutes when running for the first time (up to 13 minutes on a 4 core, 32gb memory Ubuntu setup)
    - Periodically check the logs for the smoke test process by looking for the string "smoketest done” in the docker logs:
    
    ```bash
    docker logs orchestrator
    ```
    

<aside>
❗ **It’s recommended that a smoke test is run once with the docker environment, and subsequent smoke tests are run after running the command** `make stop-smoketest`

</aside>

At this point you have a full-fledged testnet running locally.  If you’d like to explore the docker setup and build/run tests, read our **Testing 101** document.

## ZetaChain Architecture

<aside>
❗ **Make sure to read the ZetaChain [Architecture Overview](https://www.zetachain.com/docs/architecture/overview/) for a 10,000ft view of how the major components fit.**

</aside>

<aside>
❗ **The ZetaChain node is written in Go.  In case readers have little exposure to the language, but are versed in others, they are encouraged to read content from [Resources for the Go language](https://www.notion.so/Resources-for-the-Go-language-91fc1a4c56b84720be19b83667722327?pvs=21)   to quickly get a up to speed on the features and syntax.**

</aside>

### Key Architecture Patterns

ZetaChain is a [Cosmos SDK project](https://docs.cosmos.network/v0.50/learn/beginner/app-anatomy) at heart. That means it essentially relies on patterns of:

- Composable units of functionality with handlers for updating dedicated parts of state (”[Modules](https://www.notion.so/Project-101-44d769e6aae4459d85b91ddbba34ae81?pvs=21)”)
- Getters and Setters for each module’s state (”[Keepers](https://www.notion.so/Project-101-44d769e6aae4459d85b91ddbba34ae81?pvs=21)”)
- Access controls for state updates (”[Stores and Object Capabilities](https://www.notion.so/Project-101-44d769e6aae4459d85b91ddbba34ae81?pvs=21)”)
- A convention for interfacing with a consensus and networking layer through a lifecycle of events (“[CometBFT](https://www.notion.so/Project-101-44d769e6aae4459d85b91ddbba34ae81?pvs=21)”, ”[ABCI](https://www.notion.so/Project-101-44d769e6aae4459d85b91ddbba34ae81?pvs=21)”, “[Protobuf](https://www.notion.so/Project-101-44d769e6aae4459d85b91ddbba34ae81?pvs=21)”)
- Handlers to modify state according to transaction payloads and RPC requests (”[Messages](https://www.notion.so/Project-101-44d769e6aae4459d85b91ddbba34ae81?pvs=21)”, and “[Queries](https://www.notion.so/Project-101-44d769e6aae4459d85b91ddbba34ae81?pvs=21)”)

The subsequent sections describe these core concepts used within the ZetaChain architecture. 

**Further References**

- Cosmos app architecture overview: [https://docs.cosmos.network/main/learn/beginner/app-anatomy](https://docs.cosmos.network/main/learn/beginner/app-anatomy)
- Interchain Blockchain App Architecture: [https://tutorials.cosmos.network/academy/2-cosmos-concepts/1-architecture.html](https://tutorials.cosmos.network/academy/2-cosmos-concepts/1-architecture.html)

### Modules

Modules are state machines, combined with the data they store, and methods for updating that state. They process the data routed by the [Module Manager](https://docs.cosmos.network/v0.50/build/building-modules/module-manager) according to on-chain messages and RPC queries received by CometBFT.

Each module is comprised of:

1. [Module interface](https://docs.cosmos.network/main/learn/beginner/app-anatomy#application-module-interface) and implementation methods
2. `[Msg` services](https://docs.cosmos.network/main/learn/beginner/app-anatomy#msg-services)
    - These handle messages w/ service methods defined in Protobuf files
    - These services use their module’s Keeper to update module state
3. `[gRPC` services](https://docs.cosmos.network/main/learn/beginner/app-anatomy#grpc-query-services)
    - These provide an external interface to retrieve app state from [RPC calls](https://docs.cosmos.network/v0.50/learn/advanced/grpc_rest)
    - handle RPC calls with service methods defined in Protobuf files
4. A [Keeper](https://www.notion.so/Project-101-44d769e6aae4459d85b91ddbba34ae81?pvs=21), which provides getters/setters for module state
    - The Keeper provides [some guarantees](https://docs.cosmos.network/v0.50/learn/advanced/ocap) about who can access their state
5. [CLI commands](https://docs.cosmos.network/main/learn/beginner/app-anatomy#command-line-grpc-services-and-rest-interfaces)
    - Allow clients to query or transact with full nodes from the command line
        - Defined in `x/MODULE_NAME/client/cli/tx.go` and `x/MODULE_NAME/client/cli/query.go` files

### Keepers

[Keepers](https://docs.cosmos.network/v0.50/learn/beginner/app-anatomy#keeper) are the way in which modules can store and [control access to a module’s store](https://docs.cosmos.network/v0.50/learn/advanced/ocap). It is the object in a Cosmos SDK module that abstracts the interactions with a module’s store. For example, the `fungible` module contains information about foreign coins, and its keeper exposes the methods to create/read/update/delete foreign coin objects in the blockchain.

- Keepers should only ever be able to access the state through other Keepers they have references to
- Each module defines a Keeper’s type in `x/MODULE_NAME/keeper/keeper.go`

**************************Further References**************************

- [https://docs.cosmos.network/v0.50/build/building-modules/keeper](https://docs.cosmos.network/v0.50/build/building-modules/keeper)

### Stores and Object Capabilities

Each module defines a store.  To provide for a sane access model in the face of potentially malicious state handling that defines what operations a module can perform on relevant stores.

### CometBFT

[CometBFT](https://github.com/cometbft/cometbft/tree/main/spec) is the consensus engine and application interface. It is a fork of the [Tendermint](https://docs.tendermint.com/) consensus protocol. It defines the underlying blockchain such that it is Byzantine Fault Tolerant (BFT).

<aside>
❗ **The version of CometBFT used in this project is v0.34.28. Be aware of the version of documentation you are viewing.**

</aside>

- Defines ABCI methods that interface with the application, such as:
    - InitChain
    - CheckTx
    - DeliverTx
    - BeginBlock
    - EndBlock

**Further References**

- The CometBFT Consensus Algorithm Spec: [https://github.com/cometbft/cometbft/blob/main/spec/consensus/consensus.md](https://github.com/cometbft/cometbft/blob/main/spec/consensus/consensus.md)

### ABCI

This is the interface between the consensus engine (Tendermin/CometBFT) and the state machine. It provides an interface which can be consumed by different programming languages.

<aside>
❗ **The version of CometBFT used in this project (v0.34.28) does not use the ABCI++ (v2.0) interface, only the previous version of ABCI. Keep aware as the two versions are similarly-named**

</aside>

- When messages and queries are received from CometBFT, the consensus engine can use the ABCI to trigger events on the app’s state machine
- ABCI defined in the module, ex. `x/MODULE_NAME/abci.go`

**************************Further References**************************

- Intro ABCI: [https://github.com/cometbft/cometbft/blob/v0.34.28/docs/introduction/what-is-cometbft.md#abci-overview](https://github.com/cometbft/cometbft/blob/v0.34.28/docs/introduction/what-is-cometbft.md#abci-overview)
- CometBFT ABCI Spec: [https://github.com/cometbft/cometbft/blob/v0.34.28/spec/abci/abci.md](https://github.com/cometbft/cometbft/blob/v0.34.28/spec/abci/abci.md)

### Protobuf

Protobuf provides a way of defining and interfacing to handlers for on-chain messages and RPC calls.

- Modules define their Protobuf definitions in `proto/MODULE_NAME/TYPE.proto`

<aside>
❗ I**f you update any of the `.proto` files, you must regenerate the resulting `Msg` and `gRPC` services with:**

```
make proto
```

</aside>

**************************Further Links**************************

- Protocol Buffers in 2 Minutes: [https://medium.com/nerd-for-tech/protocol-buffers-in-two-minutes-6b8f908efe5](https://medium.com/nerd-for-tech/protocol-buffers-in-two-minutes-6b8f908efe5)
- Protobuf documention: [https://protobuf.dev/](https://protobuf.dev/)

### Messages

These are [handlers for the messages](https://docs.cosmos.network/main/learn/beginner/app-anatomy#msg-services) contained inside of on-chain transactions. They are specified with Protobuf and their handlers are defined in their respective modules. 

- Messages modify Keeper state
- Message handlers for each custom module are stored in `x/MODULE_NAME/keeper/` directory

![Message lifecycle.excalidraw.png](Project%20101%2044d769e6aae4459d85b91ddbba34ae81/Message_lifecycle.excalidraw.png)

### Queries

Queries are handlers for [retrieving chain state](https://docs.cosmos.network/main/learn/beginner/app-anatomy#grpc-query-services) through RPC ([and other methods](https://docs.cosmos.network/main/learn/advanced/grpc_rest#rest-server)).  They are specified with Protobuf and their handlers are defined in their respective modules. 

- Queries do not modify Keeper state
- Query handlers for each custom modules are stored in `x/MODULE_NAME/keeper/` directory

![query lifecycle.excalidraw.png](Project%20101%2044d769e6aae4459d85b91ddbba34ae81/query_lifecycle.excalidraw.png)

## Essential Folder Breakdown

The core of this codebase is contained within:

- `app/` : Cosmos SDK custom app (ZetaChain) interfaces and implementations
- `cmd/` : The scaffolding for handling commands through the CLI, and the entry point for generating the `zetacored` and `zetaclientd` binaries
- `common/` : helpers and methods used throughout the app, custom modules, and tests
- `proto/` : protobuf definitions for [Messages](https://docs.cosmos.network/v0.50/build/building-modules/msg-services) and [Queries](https://docs.cosmos.network/v0.50/build/building-modules/query-services)
- `x/`: custom Cosmos SDK [modules](https://www.zetachain.com/docs/architecture/overview/) referenced in the ZetaChain app
- `zetaclient/` : this is the off-chain ZetaClient server used for querying data from targets chain and creating messages and signing cross chain transactions
- `server/` : CLI interface for starting the ZetaCore/Blockchain processes, ultimately used by `zetacored`

## Resources for the Go language

- Go in X minutes: [https://learnxinyminutes.com/docs/go/](https://learnxinyminutes.com/docs/go/)
- Go by Example: [https://gobyexample.com/](https://gobyexample.com/)
- Go memory model: [https://go.dev/ref/mem](https://go.dev/ref/mem)
