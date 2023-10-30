<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [Zetachain 101](#zetachain-101)
  - [What is unique about Zetachain](#what-is-unique-about-zetachain)
    - [H-h-hold on, what's an Omni-chain? How is that different than bridge/multi-chain?](#h-h-hold-on-whats-an-omni-chain-how-is-that-different-than-bridgemulti-chain)
    - [The Omni-chain approach.](#the-omni-chain-approach)
      - [How does that work, and why is this important?](#how-does-that-work-and-why-is-this-important)
    - [ZRC20](#zrc20)
  - [How do you build on Zetachain?](#how-do-you-build-on-zetachain)
    - [Cross-chain messaging (for existing applications)](#cross-chain-messaging-for-existing-applications)
      - [Gas fees when using cross-chain messaging](#gas-fees-when-using-cross-chain-messaging)
    - [Omni chain contracts (for new applications)](#omni-chain-contracts-for-new-applications)
      - [Overview:](#overview)
      - [Further reading:](#further-reading)
    - [Why would you use omni chain contracts?](#why-would-you-use-omni-chain-contracts)
    - [Summary](#summary)
    - [Similar protocols](#similar-protocols)
    - [Further reading](#further-reading-1)
  - [Architecture](#architecture)
    - [`crosschain` module](#crosschain-module)
      - [Overview](#overview-1)
      - [Messages](#messages)
    - [`emissions` module](#emissions-module)
    - [`fungible` module](#fungible-module)
      - [Overview](#overview-2)
      - [Messages](#messages-1)
    - [`observer` module](#observer-module)
      - [Overview](#overview-3)
    - [Messages](#messages-2)
  - [Security Model](#security-model)

<!-- TOC end -->

## Zetachain 101

### What is unique about Zetachain

Zetachain stands out from other L1s because it's specifically designed from scratch to support on-chain contracts effectively, facilitating ease of use in a multi-chain environment. Their approach method is heavily centered around decentralization and trustlessness, enabling operation across numerous chains without the reliance on centralized trust.

While the concept of an omni-chain or multi-chain approach isn't new, and others have attempted it through methods like message passing, these solutions often depend on trusted verifiers or other centralized entities. 

In contrast, Zetachain is being built to be as decentralized as possible. A notable difference with conventional layer one blockchains is their tendency to retain users and developers within their own limited ecosystems, usually discouraging external integration.

On the contrary, Zetachain's model is intended to break these boundaries; they not only permit but actively encourage developers to use their platform as a springboard to reach and connect with other chains. 

Zetachain's goal is to provide an omni chain experience for both developers and users who navigate the multi-chain landscape, enabling more seamless connectivity and interactions across different blockchain networks.

#### H-h-hold on, what's an Omni-chain? How is that different than bridge/multi-chain?

You can think of it like a bridge is an example of a multi-chain application? It works **between two** different chains.

On the other hand, Omnichain means applications and dApps that work across multiple chains **simultaneously**. 

Typically, with multi-chain apps, the developer might be deploying the same application to multiple chains or similar applications; it's almost like they're copy-pasting from one place to the next. It's just a series of connected applications. 

On the other hand, an omnichain application is designed from the ground up to work across multiple chains. So what happens is you deploy your application logic in one place and interact with assets across various chains. 

This magic happens with **Omnichain Smart Contracts**. They are used to orchestrate assets/data on all chains, including non-smart contract solutions like Bitcoin and Dogecoin, on top of their messaging capabilities.

That makes it different and unique compared to pure messaging solutions like LayerZero and Axelar.


#### The Omni-chain approach.

The most significant (and obvious) benefit here is that it eliminates the need for things like wrapped assets, bridges, streamlining cross-chain assets, and data transfers. But how?

Zetachain has abstracted most of the complications of this in a way to make it easier for users and for developers who don't have to worry about wrapping assets and bridging and that kind of work.

All of that happens thanks to the ZRC20 token, an abstraction of a token or a native asset on an external chain.

Because on Zetachain, there are no wrapped coins, you're actually getting the original asset on the new blockchain if you're swapping. Meaning assets are never at risk in their system.

##### How does that work, and why is this important?

When you're transferring value, instead of locking assets in a vault-like in Wormhole or the Ronin incident, and then having some synthetic asset on the other destination that's backed by that's leaving all of that vault at risk. 

For Zetachain, it looks more like a two-sided swap. You would swap X for data that's burned, and then on the destination that's minted, and then traded for your target asset.

However, from a user's point of view, it's really just all that's kind of managed in the background by app developers and zeta chain. 

So to user, all you're doing is just sending a transaction, and you get the result.

There is no wrapped coin, and you get the original asset on the new blockchain if you're swapping.

#### ZRC20

The staple of Zetachain's omnichain smart contract platform is the novel [ZRC-20](https://www.zetachain.com/docs/developers/omnichain/zrc-20/) token standard, based on ERC-20.

You can think of it as an abstraction of a token or native asset on an external chain.

In a broad sense, ZRC-20 tokens are an extension of the ERC20 tokens present in the Ethereum ecosystem. 

**Characteristics:**

- ZRC-20 tokens are an enhancement of Ethereum's ERC-20 standard.
- They can manage assets on all chains connected to ZetaChain.
- ZRC-20 can represent any fungible token, including those from other chains (e.g., Bitcoin, Dogecoin, ERC-20 tokens on other networks) and various chain-specific gas assets.

**Technical Interface:**

- ZRC-20 is based on the ERC-20 standard with three additional functions and events for handling Cross-Chain Transactions (CCTXs).
- The interface includes typical ERC-20 functions such as totalSupply, balanceOf, transfer, etc., with additional functions like deposit, burn, withdraw, and associated events for cross-chain operations.
- IZRC20 and zContract interfaces specify these additional functionalities.

**Functionality:**

- Deposit: Users can send assets to ZetaChain's TSS address on a connected chain. The deposit function is triggered, and the deposited assets are credited to the depositor's address. If a transaction message contains data, DepositAndCall is invoked to forward this data to a target contract on the zEVM.
- Withdraw: Similar to the transfer function, but involves burning the token amount and triggering a Withdrawal event. This leads to a CCTX, enabling the movement of tokens out of ZetaChain.


### How do you build on Zetachain?

#### Cross-chain messaging (for existing applications)

**Concept & Deployment:**

- Cross-chain message passing involves deploying CCM-enabled contracts on different blockchains.
- These contracts can send arbitrary data and value between each other.

**Functioning:**

- Users interact with CCM-enabled contracts on any connected chain.
- These contracts use a Connector API to send messages.
- ZetaChain is a relay that forwards these messages to the intended destination chain.
- At the destination, another CCM-enabled contract receives and processes the message via the Connector API.

**Data Handling & State Management:**
The system manages the state across various CCM-enabled contracts on different chains.
CCM is suitable for applications requiring unidirectional, asynchronous interactions and does not require a unified state.

**Advantages:**

- Versatility: CCM can handle any data type, leaving data processing to the respective contracts.
- General-Purpose Utility: CCM is a robust solution for adding cross-chain features to existing applications.
- Existing Liquidity Usage: It can utilize existing liquidity (like Uniswap pools) on various chains. Transactions involve burning/minting ZETA tokens through ZetaChain, - which may be more complex and gas-intensive but doesn't depend on ZetaChain's liquidity.
- Secure Value Transfers: Using ZETA burn/mint functions enables value transfer applications without the need to bridge or wrap assets, reducing user risk.

**Usage Considerations:**

- The CCM approach might involve more transactions and higher gas costs than other methods.
- It's an effective method for applications where complex, chain-specific transactions and states are not central concerns.

##### Gas fees when using cross-chain messaging

Fees are paid in ZETA tokens, which are transferred to a Connector contract on a participating blockchain. These fees are used for:
1. Validator/Staker/Ecosystem Pools: Rewarding those who maintain the network.
2. Gas on Destination Chain: Covering the transaction costs on the recipient blockchain.

When sending a cross-chain message, you're paying two types of fees:
1. Outbound Gas Fee:
    - Depends on the destination chain’s gas prices, the user-set gas limit, and token prices in ZetaChain's liquidity pools.
    - It's calculated dynamically.
2. Protocol Fee:
    - A fixed value as per ZetaChain’s source code.
3. Example Fee Structure
    - Fees are in ZETA tokens, specific to the destination chain.
    - Calculated based on a gas limit of 500,000.
4. Approaches to Paying Fees
    - Sending ZETA to the Connector:
      - Users must have sufficient ZETA and approve the transaction.
      - Example: sendMessage function where users transfer ZETA to the connector contract.
    - Pay With ZETA From the Contract:
      - Easier for users as they don’t need ZETA.
      - More complex for developers; requires the contract to have enough ZETA.
      - Example: sendMessage where the contract handles ZETA payment.
    - Pay With Any Token and Swap to ZETA:
      - More complex due to the need for a swapping mechanism and handling market prices.
      - Convenient for users; can use any token, with ZETA conversion happening in the background.
      - Utilize functions like getZetaFromEth for swapping.
      - Example: sendMessage where any token is accepted and internally swapped for ZETA.

#### Omni chain contracts (for new applications)

##### Overview:

**Concept & Deployment:**

- Omnichain contracts are a type of smart contract deployed on ZetaChain.
- Only one deployment on ZetaChain is required; no separate contracts are needed on connected blockchains.
- It can be called from any connected chain

**Functionality:**
- For a contract to be considered omnichain it must inherit from the zContract interface and implement the onCrossChainCall function.
- Users can transfer assets or assets with a message to a ZetaChain address using a Threshold Signature Scheme (TSS).
- Transfers can either:
  - Make the asset available to the omnichain contract, or
  - Trigger the omnichain contract with the included message data if both asset and message are sent.


**Token Representation & Asset Transfers:**

- Assets transferred to a TSS address are represented as ZRC-20 tokens, an extension of ERC-20.
- Omnichain contracts support:
  - Transfer of native gas assets (e.g., ETH, BNB, MATIC) across chains.
  - Transfer of ERC-20 tokens across chains.

**Advantages:**
- Simplifies integration with existing applications like Uniswap, Curve, Aave, etc., through minimal modifications to support ZRC-20.
- Able to support "platforms" like Bitcoin or Dogecoin, which lack native smart contract functionality.
- Generally, less gas-intensive than other methods due to the lack of logic/state processing on foreign chains.
- Enables direct, single-step trading of native assets without bridges or wrapping.
- Exception handling is streamlined, as interactions are confined to standard ERC-20/contract actions.


**Key Features:**

- Entire dApp state can be managed within a single omnichain contract.
- Simplifies trading and interaction steps, enhancing user experience and efficiency.
- Cost-effective in terms of gas usage compared to other message-passing methods.


##### Further reading:
- [Example Omnichain Contracts](https://github.com/zeta-chain/example-contracts)
- [Common pitfalls for Cosmos Contracts](https://secure-contracts.com/not-so-smart-contracts/cosmos/index.html)
- [A developer's guide to building secure applications on Cosmos](https://www.zellic.io/blog/exploring-cosmos-a-security-primer)


#### Why would you use omni chain contracts?

Any EVM smart contract you were considering making multi-chain could be an omni-chain contract.

The most significant benefit comes from deploying and interacting with the main logic in one location because it makes the operations much easier. 

You're managing one application instead of managing five applications, right? 

It also allows you to streamline the user experience a little bit; the user may not need to jump between four or five different chains to do something.

#### Summary

So, in summary, omni-chain smart contracts are different from cross-chain messaging in that they allow for transferring arbitrary data and value between multiple connected chains, while cross-chain messaging is limited to transferring data and value within a single chain.

#### Similar protocols

Besides everything mentioned, Zetachain is similar to any other Ethermint-enabled Cosmos chain.

Ethermint enabled just means that there are two chains, one is the actual cosmos chain, which is Zetachain in this case.

The other is an EVM-compatible chain, which is called EVM.

The most notable similar example is the Cronos chain, where Cronos chain is the EVM compatibility layer chain (aka the zEVM), and the crypto.com chain is the actual cosmos chain behind the Cronos chain.

#### Further reading
- [Building with Zetachain](https://www.zetachain.com/docs/developers/overview/)
- [Omnichain Contract Template](https://www.zetachain.com/docs/developers/template/)
- [Building Omnichain contracts](https://www.zetachain.com/docs/developers/omnichain/overview/)
- [Integrating existing applications](https://www.zetachain.com/docs/developers/cross-chain-messaging/examples/hello-world/)
- [Top 5 security vulnerabilities Cosmos Developers need to watch out for ](https://www.halborn.com/blog/post/top-5-security-vulnerabilities-cosmos-developers-need-to-watch-out-for)


### Architecture

#### `crosschain` module

##### Overview

The `crosschain` module manages cross-chain transactions through a network of observers and a systematic voting process. 

This structure is used to secure and accurately execute transactions across different blockchains, with specific processes for handling both inbound and outbound transactions.

![](https://i.imgur.com/6GoadJU.png)

**Key Components:**

**1. Observers:**

- Main actors in the Crosschain module.
- Run an off-chain program (zetaclient) to monitor:
- Inbound transactions on connected blockchains.
- Outbound transactions on both ZetaChain and connected chains.


**2. Voting Process:**

- Observers vote on observed transactions.
- A ballot is created upon the first vote for a transaction.
- Observers cast votes, and when enough votes are cast (based on the BallotThreshold), the ballot is finalized.
- The last vote that finalizes the ballot triggers the execution of the cross-chain transaction and pays its gas costs.
- Votes after finalization are discarded.

**3. Inbound Transaction Handling:**

- Inbound CCTXs observed on connected chains.
- Observers vote using MsgVoteOnObservedInboundTx.
- On finalization:
    - If ZetaChain is the destination and no message is in the CCTX, ZRC20 tokens are deposited into a ZetaChain account.
    - If the CCTX contains a message, ZRC20 tokens are deposited, and a ZetaChain contract is called using details from the message.
    - If the destination is not ZetaChain, the transaction is marked "pending outbound" for outbound processing.

**4. Outbound Transaction Processing:**

- Pending Outbound:
    - Observers watch for these on ZetaChain.
     -They participate in a TSS keysign ceremony, then broadcast the signed transaction to connected blockchains.
- Observed Outbound:
    - Observers monitor these on connected chains.
    - Confirmed transactions are voted on in ZetaChain using VoteOnObservedOutboundTx.
    - After meeting the vote threshold and finalization, the transaction's status updates to final.

**5. Permissions:**
- Certain actions, like creating TSS voters or voting on observed transactions, are permitted to the admin policy account or observer validators.

**6. State Management:**

- The module's state includes lists of outbound transactions, chain nonces, last chain heights, cross-chain transactions, and a mapping between inbound transactions and CCTXs.
- Also stores the TSS key and observer-submitted gas prices on connected chains.

##### Messages

The `crosschain` module uses a variety of messages for managing cross-chain transactions and network operations. These messages involve various functionalities like tracking transactions, voting mechanisms, managing gas prices, and working with TSS (Threshold Signature Scheme) keys.

![](https://imgur.com/jgxmuR2.png)


Explanation:
- Admin Policy Account (Admin) and Observer Validators (Observer) can add to or remove from the Outbound Tx Tracker (Tracker) using MsgAddToOutTxTracker and MsgRemoveFromOutTxTracker respectively.
- Node Accounts (Node) can trigger a TSS key creation vote with MsgCreateTSSVoter.
- Observer Validators can submit gas price information with MsgGasPriceVoter and vote on observed transactions with MsgVoteOnObservedOutboundTx and MsgVoteOnObservedInboundTx.
- Lastly, Admin can whitelist ERC20 tokens with MsgWhitelistERC20 and update the TSS address with MsgUpdateTssAddress.

**Messages and Their Functions:**

**1. MsgAddToOutTxTracker:**
- Adds a new record to the outbound transaction tracker.
- Authorized users: Admin policy account and observer validators.

**2. MsgRemoveFromOutTxTracker:**
- Removes a record from the outbound transaction tracker.
- Authorized users: Only the admin policy account.

**3. MsgCreateTSSVoter:**
- Votes on creating a TSS key and recording its details (public key, participant addresses, etc.).
- Authorized users: Only node accounts.
- Successful votes record the TSS key on-chain and update the keygen status.

**4. MsgGasPriceVoter:**
- Submits information about gas prices of connected chains.
- Authorized users: Only observer validators.
- Records each validator's submitted gas price separately, updating a median index.

**5. MsgNonceVoter:**
- It's deprecated

**6. MsgVoteOnObservedOutboundTx:**
- Casts a vote on an observed outbound transaction.
- Authorized users: Only observer validators.
- Voting affects how outbound transactions are processed, including cases of failed observations, status updates, and handling of revert transactions.

**7. MsgVoteOnObservedInboundTx:**
- Casts a vote on an observed inbound transaction.
- Authorized users: Only observer validators.
- Involves deposit methods, contract executions, and processing inbound transactions for ZetaChain and connected chains.

**8. MsgWhitelistERC20:**
- Used to whitelist ERC20 tokens.
- Contains details like ERC20 address, chain ID, and token characteristics (name, symbol, decimals).

**9. MsgUpdateTssAddress:**
- Used for updating TSS addresses.
- Contains details like the creator and the TSS public key.

#### `emissions` module

The emissions module is pivotal in ensuring fair and timely reward distribution to various network participants, which is fundamental for network security and incentivization.
The current focus on rewarding validators every block, with TSS signers and observers accumulating rewards, suggests a prioritization that might reflect the network's operational or security strategies.

![](https://i.imgur.com/WzttDLB.png)


**Functionality:**

**1. Rewards Distribution:**

- Currently, the module distributes rewards only to validators for each block they validate.
- Rewards for TSS signers and observers, though accumulated, are not distributed immediately but stored in their respective pools.

**2. Implementation:**

The distribution mechanism of these rewards is executed during the 'begin blocker' stage of block processing.

**3. Parameters Tracking:**
- The module maintains several key parameters to calculate the rewards effectively:
    - Maximum Bond Factor: Likely a parameter determining the upper limit of bonding (staking) rewards.
    - Minimum Bond Factor: Sets the lower limit for bonding rewards.
    - Average Block Time: The typical time for a new block creation in the blockchain.
    - Target Bond Ratio: Desired ratio of bonded (staked) tokens to the total token supply for stability and security.
    - Validator Emission Percentage: Percentage of total rewards designated for validators.
    - Observer Emission Percentage: Share of total rewards allocated to observers.
    - TSS Signer Emission Percentage: Portion of rewards given to participants in the TSS.
    - Duration Factor Constant: Possibly a factor used in calculating rewards based on the time or duration of certain activities.


#### `fungible` module

##### Overview

![](https://imgur.com/0iZH71o.png)

The fungible module on ZetaChain is used to facilitate the interaction and integration of fungible tokens from external chains within the ZetaChain ecosystem. Its primary functions and state management are outlined below:


**1. Deployment of "Foreign" Coins:**
- The module enables the introduction and deployment of fungible tokens from connected blockchains (referred to as "foreign coins") onto the ZetaChain platform.
- These foreign coins are represented and managed as ZRC20 tokens within ZetaChain.

**2. ZRC20 Contract Deployment and Management:**

- Upon deploying a foreign coin on ZetaChain, the corresponding ZRC20 contract is created.
- This process includes creating a liquidity pool for the foreign coin, adding liquidity to this pool, and then listing the foreign coin in the module's register of managed tokens.

**3. Integration with System Contracts and Uniswap:**
- The module supports deploying key system contracts and functionalities such as Uniswap and wrapped ZETA (a representation of ZetaChain's native token on external chains).
- These integrations enable seamless liquidity and exchange functionalities within the ZetaChain ecosystem.

**4. Omnichain Smart Contract Interactions:**
- The module allows for the depositing of ZRC20 tokens to omnichain smart contracts.
- It supports specific functionalities like DepositZRC20AndCallContract and DepositZRC20, enabling ZRC20 tokens to interact across different chains via ZetaChain.

**5. System Contract Address:**
- Stores the address of the primary system contract, which is central to the module's operations and integrations across the ZetaChain and other connected blockchains.

**6. Registry of Foreign Coins:**
- Maintains a list of all foreign coins deployed on ZetaChain.

##### Messages

![](https://i.imgur.com/gd9yms3.png)

**1. MsgDeployFungibleCoinZRC20**
- Deploys a fungible coin from a connected chain as a ZRC20 token on ZetaChain.
- Process:
    - For gas coins: Deploys a ZRC20 contract, sets the contract address in the system contract, mints ZETA tokens to the module account, updates the gas pool, and adds liquidity.
    - For non-gas coins: Deploys a ZRC20 contract and adds the coin to the foreign coins list.
- Admin policy account only.
  
**2. MsgRemoveForeignCoin**
- Removes a coin from the list of foreign coins.
- Admin policy account only.

**3. MsgUpdateSystemContract**
- Updates the address of the system contract.

**4. MsgUpdateZRC20WithdrawFee**
- Adjusts the withdrawal fee for a specific ZRC20 token.

**5. MsgUpdateContractBytecode**
- Updates a contract’s bytecode (limited to ZRC20 or WZeta connector contracts).
- New bytecode must maintain the same storage layout as the original, allowing for new variables but not the removal of existing ones.

**6. MsgUpdateZRC20PausedStatus**
- Changes the paused status of one or more ZRC20 tokens.

#### `observer` module

##### Overview

**Key functionality**

- Tracks voting ballots.
- Maintains a mapping between blockchain chains and observer accounts.
- Records a list of supported connected chains.
- Manages various parameters including:
    - Core Parameters: Contract addresses, outbound transaction schedule intervals, etc.
    - Observer Parameters: Ballot threshold, minimum observer delegation, etc.
    - Admin Policy Parameters.

**Ballots Management:**
- For voting on inbound (incoming) and outbound (outgoing) transactions.
- Includes Create, Read, Update, and Delete (CRUD) actions for ballots, along with functions to verify ballot finalization.

**Integration in Other Modules:**
- The ballot system is integral to modules like the crosschain module, where it is used by observer validators in voting on transactions.

**Role of Observer Validators:**
- These validators operate both zetaclient and zetacore
- They have the authority to vote on inbound and outbound cross-chain transactions.

**Chain-Observer Account Mappings:**
- Set during the network's genesis.
- Critical in the crosschain module to identify and authorize observer validators for transactions associated with particularly connected chains.

#### Messages

![](https://i.imgur.com/7RDC0RR.png)

**1. MsgAddObserver**
- Adds a new observer to the store.
- Resets keygen.
- Pauses inbound transactions for new TSS generation.
- Requires an admin policy account.

**2. MsgUpdateCoreParams**

- Updates core parameters for a specific chain (e.g., confirmation count, transaction schedule interval, contract addresses).
- Throws error if the chain ID isn't supported.
- Limited to the admin policy account.

**3. MsgAddBlameVote**
- Used to add a blame vote, indicating fault or error.

**4. MsgUpdateCrosschainFlags**
- Updates flags related to crosschain operations (e.g., enabling/disabling inbound/outbound transactions, gas price increase flags).
- Restricted to the admin policy account.

**5. MsgUpdateKeygen**
- Updates the block height for key generation and sets status to "pending keygen".
- Requires an admin policy account.

**6. MsgAddBlockHeader**
- Adds a block header to the store based on majority voting by observers.


### Security Model

ZetaChain is a Proof of Stake (PoS) blockchain, which can connect to any external blockchain or layer in a decentralized, trustless, permissionless way - without a single point of failure.

The ZetaChain architecture consists of validators, observers, and signers.

**Validators**

- Utilizes the Tendermint consensus protocol, a Byzantine Fault Tolerant algorithm.
- Votes on block proposals with voting power proportional to their staked ZETA coins.
- Must remain online continuously for block production.
- Identified by their unique consensus public key.
- Rewarded with block rewards and transaction fees for their service.

**Observers**

- Essential for ZetaChain consensus.
- Monitor external chains for relevant transactions/events/states.
- Comprised of sequencers and verifiers.
- Sequencers: Detect relevant external transactions/events/states and relay this information to verifiers.
- Verifiers: Verify the data and vote on ZetaChain for consensus.
- At least one honest sequencer is required for network liveness.

**Decentralized Transaction Signing: In a distributed fashion, mutating states on external blockchains is authenticated & secured by leaderless Threshold Signature Scheme (TSS)**
- Zetachain's TSS is forked from [Binance](https://github.com/bnb-chain/tss-lib)
  - Previously Known Issues : [CVE-2022-47930](https://www.cve.org/CVERecord?id=CVE-2022-47930) , [CVE-2022-47931](https://www.cve.org/CVERecord?id=CVE-2022-47931) , [CVE-2023-26556](https://www.cve.org/CVERecord?id=CVE-2023-26556) , [CVE-2023-26557](https://www.cve.org/CVERecord?id=CVE-2023-26557)
  - Affected Projects by Binance TSS Lib Vulnerability : [Thorchain Halt](https://twitter.com/THORChain/status/1691793526382789044)
- Hold standard ECDSA/EdDSA keys for interacting with external chains.
- Keys are distributed so that a supermajority is needed to sign on behalf of ZetaChain.
- Ensure that no single entity can sign messages on behalf of ZetaChain to external chains.
- Bonded stakes, coupled with positive/negative incentives, ensure economic safety.
- The decentralized transaction signing process initiates smart contract actions in a way that does not reveal any secrets to participating nodes. This is what makes non-smart chain connectivity possible.
