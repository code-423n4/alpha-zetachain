- [Zetachain Previous Cosmos Audits Explained](#zetachain-previous-cosmos-audits-explained)
    - [Zellic 3.1: Any ZetaSent events are processed regardless of what contract emits them (critical)](#zellic-31-any-zetasent-events-are-processed-regardless-of-what-contract-emits-them-critical)
    - [Zellic 3.2: Bonded validators can trigger reverts for successful transactions (critical)](#zellic-32-bonded-validators-can-trigger-reverts-for-successful-transactions-critical)
    - [Zellic 3.3: Sending ZETA to a Bitcoin network results in BTC being sent instead (critical)](#zellic-33-sending-zeta-to-a-bitcoin-network-results-in-btc-being-sent-instead-critical)
  - [Zellic 3.4 Race condition in Bitcoin client leads to double spend (critical)](#zellic-34-race-condition-in-bitcoin-client-leads-to-double-spend-critical)
    - [Zellic 3.5/Halborn 04: Not waiting for minimum number of block confirmations results in double spend (critical)](#zellic-35halborn-04-not-waiting-for-minimum-number-of-block-confirmations-results-in-double-spend-critical)
    - [Zellic 3.6: Multiple events in the same transaction causes loss of funds and chain halting](#zellic-36-multiple-events-in-the-same-transaction-causes-loss-of-funds-and-chain-halting)
    - [Zellic 3.7: Missing authentication when adding node keys (critical)](#zellic-37-missing-authentication-when-adding-node-keys-critical)
    - [Zellic 3.8: Missing nil check when parsing client event (high)](#zellic-38-missing-nil-check-when-parsing-client-event-high)
    - [Zellic 3.9: Case-sensitive address check allows for double signing](#zellic-39-case-sensitive-address-check-allows-for-double-signing)
    - [Zellic 3.10: No panic handler in Zetaclient may halt cross-chain communication](#zellic-310-no-panic-handler-in-zetaclient-may-halt-cross-chain-communication)
    - [Zellic 3.11: Ethermint Ante handler bypass (high)](#zellic-311-ethermint-ante-handler-bypass-high)
    - [Zellic 3.12: Unbonded validators prevent the TSS vote from passing](#zellic-312-unbonded-validators-prevent-the-tss-vote-from-passing)
    - [Halborn 01: Zeta Supply does not track assets correctly (critical)](#halborn-01-zeta-supply-does-not-track-assets-correctly-critical)
    - [Halborn 02/12: Integers Overflows/Truncations Cause Havoc (critical)](#halborn-0212-integers-overflowstruncations-cause-havoc-critical)
    - [Halborn 03: Possible Division by Zero could cause chain halt due to panic (critical)](#halborn-03-possible-division-by-zero-could-cause-chain-halt-due-to-panic-critical)
    - [Halborn 05: Lack of mechanism to limit the supply of zeta (high)](#halborn-05-lack-of-mechanism-to-limit-the-supply-of-zeta-high)
    - [Halborn 06: Price Manipulation and Denial of Service via UpdatePrices Function (high)](#halborn-06-price-manipulation-and-denial-of-service-via-updateprices-function-high)
    - [Halborn 07: Error Condition for Key Signing is Unchecked](#halborn-07-error-condition-for-key-signing-is-unchecked)
    - [Halborn 08: Iteration over maps - non-determinims (medium)](#halborn-08-iteration-over-maps---non-determinims-medium)
    - [Halborn 09: Sybil attack risk due to use of median gas votes for setting gas price (medium)](#halborn-09-sybil-attack-risk-due-to-use-of-median-gas-votes-for-setting-gas-price-medium)
    - [Halborn 10: Malicious Gas Price Voting - Denial of Service by setting large gas prices for evm networks (medium)](#halborn-10-malicious-gas-price-voting---denial-of-service-by-setting-large-gas-prices-for-evm-networks-medium)
    - [Halborn 11:  Malicious Gas Price Voting - Denial of Service by setting gas prices for bitcoin (medium)](#halborn-11--malicious-gas-price-voting---denial-of-service-by-setting-gas-prices-for-bitcoin-medium)
    - [Halborn 13: Reliance on UniswapV2 Pools for Prices Exposes Zetachain to price manipulation risk  (low)](#halborn-13-reliance-on-uniswapv2-pools-for-prices-exposes-zetachain-to-price-manipulation-risk--low)
    - [Halborn 14: Arbitrary Minting of Zeta via MintZetaToEVMAccount Function (low)](#halborn-14-arbitrary-minting-of-zeta-via-mintzetatoevmaccount-function-low)
    - [Halborn 16: Use of Vulnerable Cosmos SDK Version (low)](#halborn-16-use-of-vulnerable-cosmos-sdk-version-low)
    - [Halborn 17: ValidateBasic incomplete for some message types](#halborn-17-validatebasic-incomplete-for-some-message-types)
- [Smart Contract Audits](#smart-contract-audits)
    - [Halborn 04 - ZRC20 Lacks Resistance to ERC20 Race Condition Issue](#halborn-04---zrc20-lacks-resistance-to-erc20-race-condition-issue)
    - [Peckshield 1/Quantum Brief 1/Halborn 01/Halborn 02: Accommodation of Non-ERC20-Compliant Tokens (low)](#peckshield-1quantum-brief-1halborn-01halborn-02-accommodation-of-non-erc20-compliant-tokens-low)
    - [Peckshield 2: Trust Issue of Admin Keys (medium) - invalid](#peckshield-2-trust-issue-of-admin-keys-medium---invalid)
    - [Verdise 1/2: Possible Event Reordering via Reentrancy in getEthFromZeta/getZetaFromToken](#verdise-12-possible-event-reordering-via-reentrancy-in-getethfromzetagetzetafromtoken)
    - [Halborn 05: Insecure Use of Tx.Origin in the Send Function](#halborn-05-insecure-use-of-txorigin-in-the-send-function)
    - [Halborn 09: Missing slippage/min-return check on crosschain call function](#halborn-09-missing-slippagemin-return-check-on-crosschain-call-function)
- [Denial of Service Testing](#denial-of-service-testing)
    - [Halborn 01: DoS By Issuing Large Number of Cross Chain Swaps (critical)](#halborn-01-dos-by-issuing-large-number-of-cross-chain-swaps-critical)
    - [Halborn 02: Loss of Funds Due to Incorret Gas Price Listing (critical)](#halborn-02-loss-of-funds-due-to-incorret-gas-price-listing-critical)
    - [Halborn 03: Loss of Funds due to illiquid uniswap gas pools (medium)](#halborn-03-loss-of-funds-due-to-illiquid-uniswap-gas-pools-medium)
    - [Halborn 04: Possible Network Congestion due to ecosystem incentives (low)](#halborn-04-possible-network-congestion-due-to-ecosystem-incentives-low)


## Zetachain Previous Cosmos Audits Explained 
- Two previous audits of the ``zetacore`` and ``zetaclient`` were performed by Zellic and Halborn.
- The Halborn audit was recently removed from being online. I had some notes from previously reading it and tried to reconstruct it to the best of my ability.
- Below is an explanation of all the Cosmos SDK and client vulnerabilities discovered from the previous two audits in sufficient detail to understand them. 

#### Zellic 3.1: Any ZetaSent events are processed regardless of what contract emits them (critical)
- [Ethermint](https://docs.ethermint.zone/) is an EVM implementation within the Cosmos SDK ecosystem. This particular implementation is essentially the only one used by projects. Zetachain uses Ethermint for the zEVM.
- To send funds from the zEVM to another chain, a ``ZetaSent`` event is emitted by the EVM by the Connector contract. 
- The event ``orginator`` was not validated or checked in this case. The contract should have checked that the sender was the Connector contract. 
- Because this was not validated, an atacker could send this event to trigger the bridging functionality from an arbitrary contract. This allowed for arbitrary creation of events to other blockchains without actually locking the funds.

#### Zellic 3.2: Bonded validators can trigger reverts for successful transactions (critical) 
- Zetachain messages (essentially Cosmos SDK functions) for adding and removing transactions to be signed by the TSS address. 
- These are the ``MsgAddToOutTxTracker`` and ``MsgRemoveFromOutTxTracker`` messages. Only bonded validators are allowed to submit these. This is a dangerous primitive to give. 
- Since they can *add* and *remove* they can *swap* transactions in and out to get them voted on. A flow for explotation is shown below: 
    1. Transfer some ETH from Goerli back to Goerli. The two chains do not matter as long as they are both EVM chains.
    2. The event will be processed by the voting and the TSS address will sign the request to go back to the EVM chain. The funds have been delivered. 
    3. Remove the transaction using the ``MsgRemoveFromOutTxTracker`` message. At this point, Zetachain is waiting on a response (voting) that the transaction succeeded on the EVM chain. 
    4. Call ``MsgAddToOutTxTracker`` to add a transaction that had previously failed with the original nonce. 
    5. The voting process will believe that the ``removed transaction`` failed or reverted because of this. 
    6. The TSS address calls the ``onZetaRevert`` function of the contract to give a refund. 
Since the transaction is sent and a refund is provided, this results in a *double spend*.


#### Zellic 3.3: Sending ZETA to a Bitcoin network results in BTC being sent instead (critical)
- There are three different transaction types:
    1. CoinType_Zeta - The zetchain itself
    2. CoinType_Gas - Bitcoin, Dogecoin and others
    3. CoinType_ERC20 - USDT, USDC
- The TSS address is a made up of a collection of validators who own small amounts of the key. Collectively, they can sign a transaction like a multi-sig address once something is voted on. This is the ``zetaclient`` process. It determines the voting and TSS signing process. 
- When deciding to sign a ``CoinType_Gas`` transaction (bitcoin, Dogecoin, etc.), the program assumed that only Bitcoin coins were being sent to the blockchain. The bitcoin signer would see the transaction sent to the Bitcoin address and sign it. 
- However, the coin type was not verified at all. As a result, an attacker could send a little amount of ZETA to a bitcoin blockchain, which would result in the Bitcoin being sent to the users address. 
- A tiny fraction of Zeta (1/1e10) could be used to receive one BTC in return as a result.


### Zellic 3.4 Race condition in Bitcoin client leads to double spend (critical)
- The bitcoin client is used both for watching Zetachain cross-chain transactions as well as relaying transaction transactions to and from the Bitcoin chain. There is a bunch of functionality happening all at once. 
- There are four main processes running in different threads: 
    1. ``isSendOutTxProcessed``: Whether the transaction in question was submitted for relaying
    2. ``startSendScheduler``: Gets all pending cross chain transactions (CCTX) to see if they have been submitted. 
    3. ``TryProcessOutTx``: Signs and broadcasts CCTXs then adds them to a tracker on Zetachain. 
    4. ``observeOutTx``: Queries for all transactions in the crosschain module and adds them to processed queue. 
- Since these all run in different threads without any locking or communication, they are very *racy* by nature. 
- If funky ordering happens here, then it's possible that a transaction gets sent **twice**. This leads to a double spend from just a normal workflow. 


#### Zellic 3.5/Halborn 04: Not waiting for minimum number of block confirmations results in double spend (critical)
- Blockchains sometimes have soft forks that occur from different parts of the network. By default, the deepest fork is taken to be the real one. This is called a blockchain *reorg*. 
- The Zetachain was waiting for a single block confirmation before the transaction was considered valid. 
- If a reorg occurred then a transaction could happen on fork A observed by the relayer then a reorg could occur, removing the transaction. 
- This results in the user having the Bitcoin (instead of the TSS address) and the user having funds within the Zetachain ecosystem. Essentially, it creates a *double spend*. 


#### Zellic 3.6: Multiple events in the same transaction causes loss of funds and chain halting
- When transferring funds out of the zEVM onto another blockchain the ``ZetaConnectorZEVM`` contract emits events for the Zetachain Cosmos SDK module to handle. 
- To store this information, the event is internally hashed. Then, when the event is voted on, it is easy to check what is exactly being voted on. 
- As a result, *two events* in the same block with the *same parameters* will have the **same hash**. This results in two transactions being processed as a single one. This could happen accidentally in the course of action or on purpose. 
- If a user sends 5000Zeta to themselves twice, they should receive 10000Zeta. However, they will only receive 5000Zeta because of the hash collision. 
- Additionally, the *nonce* will be incremented twice even though a single transaction will be processed. Since the Ethereum receiving side enforces the *ordering* of these nonces, there will be a gap in processed transaction nonces. Since transactions cannot be processed, it results in a halting of the bridge. 

#### Zellic 3.7: Missing authentication when adding node keys (critical) 
- The function ``SetNodeKeys`` is used for supplying a public key used for TSS signing. Since the TSS address holds all of the funds as a distributed entity, it's important that this is kept secure.
- There is no verification or authentication on addition of the public key. This is a classic access control issue.
- If an attacker adds their own TSS public key, they could potentially control enough signatures to pass the threshold and sign the transactions. Or, the TSS address will not work, resulting in a denial of service. 


#### Zellic 3.8: Missing nil check when parsing client event (high) 
- The ``zetaclient`` is a Go binary that listens for incoming transactions to handle incoming and outgoing events, sign transactions and more. 
- When processing an outgoing transaction, the function ``GetChainFromChainId()`` is called. If not found, then ``nil`` is returned. 
- ``zetaclient`` calls ``Int64`` on the output of this without checking for ``nil``. An attacker is able to force this to occur by giving an invalid chain ID. 
- This would lead to an invalid conversion and a panic within the client. Since the client is incredibly important to the relaying and signing between different chains, this would halt the bridging. 


#### Zellic 3.9: Case-sensitive address check allows for double signing
- The function ``IsDuplicateSigner()`` is used to check if an address is already within a group of signers. This is used within the ``MsgCreateTSSVoter`` message used for voting on a new TSS key.
- The validation loops over a list of addresses and confirms that the current ``msg.sender`` is not one of them. If so, the votes counts. 
```
func isDuplicateSigner(creator string, signers []string) bool{
    for _,v range signers {
        if creator === v{
            return true 
        }
    }
    return false
}
```
- Within Cosmos SDK chains, addresses are alphanumeric. Both an all capitalized and all lowercase address are valid. 
- The validation above does not consider this, leading to a user being able to vote *twice*. If an attacker controlled multiple validators, this could drastically change the voting process. 

#### Zellic 3.10: No panic handler in Zetaclient may halt cross-chain communication
- The ``zetaclient`` is a crucial part of the ecosystem. Although zetachain will function fine on its own, all bridging and cross-chain functionality would be broken. 
- Having this client be resilient to crashes is important as a result. There were no protections against Go panics recovering at the time of this audit. 
- If a crash occurred, it would prevent all transactions from both the EVM and Bitcoin signers to go through. 
- One way a crash was found was sending an invalid Bitcoin address. Upon calling ``btcutil.DecodeAddress()`` with an invalid address, a crash would occur. 


#### Zellic 3.11: Ethermint Ante handler bypass (high) 
- The ``antehandler`` is the first part of the walled-garden of the Cosmos SDK messages. It performs validations such as signature validation, gas fee checks and more. To learn more on how this works, go to [docs](https://docs.cosmos.network/v0.45/modules/auth/03_antehandlers.html). 
- The Ethermint application modified the standard antehandler for their EVM purposes. However, when doing this, they failed to understand the security implications. The antehandler is only verified for the entire transaction and not any other messages nested inside of it. In particular, the antehandler was not checked on *nested* messages via ``MsgExec``. Additionally, a special extension flag made it possible to bypass the gas verification as well. For more information on these two vulnerabilities, please check out this [blog](https://jumpcrypto.com/writing/bypassing-ethermint-ante-handlers/).
- The version of Zetachain was using a vulnerable version of the Ethermint antehandler described above. The auditors used this to bypass the gas verification to breaking infinite loops to halt the blockchain.

#### Zellic 3.12: Unbonded validators prevent the TSS vote from passing
- Voting for a new TSS address is done via the ``MsgCreateTSSVoter`` call. When verifying *who* can vote, it checks to ensure that only *bonded* validators can vote. 
- When checking for the number of validators for the vote to pass, it checks the *total* amount of validators and not just *bonded* validators. This results in it counting unbonded and unbonding validators. 
- Since the amount of validators cannot be reached, the voting will never pass, since it requires a full consensus. 


#### Halborn 01: Zeta Supply does not track assets correctly (critical) 
- In the [whitepaper](https://www.zetachain.com/whitepaper.pdf), specific mention is made to keeping the *supply* of Zeta at a consistent amount. This is because the only native token on Zetachain is the ZETA token. If this could be inflated on one blockchain then used on another, then the consequences would be significant. 
- The system uses a *mint and burn* model for keeping track of Zeta between chains. Keeping these perfectly in check is important. 
- Possible to inflate amount of total Zeta via simple calls. By transferring funds from one EVM to another, one transferring chain does not *burn* the tokens. Having too many tokens in circulation would break the Uniswap pools used for trading and other security assumptions. 

#### Halborn 02/12: Integers Overflows/Truncations Cause Havoc (critical)
- Go is susceptible to number related vulnerabilities like integer overflows, underflows, sign changes and truncation.
- Multiple occurrences of overflows and signedness issues were discovered on the project.
- 02: Large block heights can overflow within the ``EndBlocker`` when changing the gas price of stuck transactions. Because this overflow occurs, the pending transactions will be increased by 20%, resulting in a network outage.
- 12: With very large block heights, a crash will occur within the querying (GRPC) of blockchain state. 
- 21: Bitcoin transaction nonces can overflow. This leads to the transactions going unprocessed. 

#### Halborn 03: Possible Division by Zero could cause chain halt due to panic (critical)
- In the ``emissions`` module, the ``Quo`` method is used to divide by the bonding ratio.
- The bounding ratio may return 0 in some cases.
- If this occurs, a panic will occur, halting the chain.

#### Halborn 05: Lack of mechanism to limit the supply of zeta (high) 
- In the [whitepaper](https://www.zetachain.com/whitepaper.pdf), specific mention is made to keeping the *supply* of Zeta at a consistent amount. This is because the only native token on Zetachain is the ZETA token. If this could be inflated on one blockchain then used on another, then the consequences would be significant. 
- If an implementation flaw is found that causes this issue, then there is no way for the other blockchains to know. According to the whitepaper, a chainlink keeper should post the total amount on every chain to ensure that the *supply* stays valid at all times. 
- The chainlink keeper was not (and has not still) been implemented. So vulnerabilities could effect the supply of Zeta to wreck havoc on the ecosystem. 

#### Halborn 06: Price Manipulation and Denial of Service via UpdatePrices Function (high) 
- The ``UpdatePrices()`` function is used to check whether a sufficient gas fee has been paid.
- However, the check occurs *after* state changing operations have occurred. Most notably, the price of the gas could be affected by the modifications. Checks after state modifications are very bad.
- The message will eventually revert because of not having enough gas. However, going this deep into the transaction to fail is less than ideal.
- According to the report, an attacker can succeed on a call to ``UpdatePrices()`` to modify the prices in the Cosmos environment. However, we're extremely confused from this because the check *is* valid just late. Hence, the state changes should revert.

#### Halborn 07: Error Condition for Key Signing is Unchecked 
- The function ``TestKeysign()`` was used for signature verification is some places. 
- The return value of this was not always checked within the ``zetaclient``. 
- As a result, an invalid signature would get flagged as invalid but execution would continue anyway as if the signature was valid.

#### Halborn 08: Iteration over maps - non-determinims (medium) 
- Consensus is the agreement of a state in a blockchain. It is important for the good being ran within a blockchain to be *deterministic* so that the same point can always be reached. 
- In Go, iterating over maps is known to be different on different versions of Go. Additionally, in newer versions, this is [intentionally randomized](https://stackoverflow.com/questions/9619479/go-what-determines-the-iteration-order-for-map-keys). 
- If the ordering of iterating over a map will change the state of a blockchain, then it's a source of *non-determinism*. This would result in a chain halt as the validators could not come to a conclusion on the state of the blockchain.

#### Halborn 09: Sybil attack risk due to use of median gas votes for setting gas price (medium)
- The gas price of networks is determined by the validators. In particular, they check various locations to vote on the gas price. 
- The voting process takes the *median* gas price. The *median* is the center of the list and NOT the average. 
- This results in massive changes in gas price. For instance, if the sent in prices are 1,2,3,200,300, the price would be 3. However, if a new vote came in, the price could rise to 200 suddenly.
- Since the average is not being used and validator power is not considered, this is vulnerable to sybil attacks as well. An attacker with control over multiple low power validators could deeper influence the vote by adding in large gas prices. Besides sybil attacks, *collusion** could occur between validators as well.
- The recommended fix for this was to use a Time Weighted Average Price (TWAP). However, this was not implemented.

#### Halborn 10: Malicious Gas Price Voting - Denial of Service by setting large gas prices for evm networks (medium)
- The gas price of networks is determined by the validators. In particular, they check various locations to vote on the gas price. 
- Validators can collude to provide gas prices that are too high for the network, resulting in a network shutdown.
- The recommended fix was to impose a *cap* on the gas price, which was implemented.

#### Halborn 11:  Malicious Gas Price Voting - Denial of Service by setting gas prices for bitcoin (medium)
- The gas price of networks is determined by the validators. In particular, they check various locations to vote on the gas price. 
- The ``zetaclient`` updates the price of gas for Bitcoin by default every 5 seconds which is the *reset* interval. Theoretically, any of the validators can vote on a gas price at any point though.
- An attacker who *races* this to get their numbers posted and posts them *very* low will create a transaction that will not get processed by any miners.
- Additionally, 

#### Halborn 13: Reliance on UniswapV2 Pools for Prices Exposes Zetachain to price manipulation risk  (low) 
- To go from one token to ZETA, UniswapV2 pools are created within the EVM ecosystems.
- UniswapV2 is known to be vulnerable to various security risks, such as frontrunning, price manipulation and depegging of stablecoins.
- The recommended fix was to use UniswapV3 instead but this was not done.

#### Halborn 14: Arbitrary Minting of Zeta via MintZetaToEVMAccount Function (low)
- The function ``MintZetaToEVMAccount`` is used for transferring ZETA from other chains to the zEVM.
- When an error occurs in this function, the protocol still returns a valid state rather than reverting. As a result, the minted tokens are still there, resulting in an increase in supply.

#### Halborn 16: Use of Vulnerable Cosmos SDK Version (low)
- Versions 0.46.10 and below are suspectible to a denial of service attack.
- Since this version of the SDK is being used by Zetachain, they are also vulnerable to the attack.

#### Halborn 17: ValidateBasic incomplete for some message types
- The ``ValidateBasic()`` function is used for verifying that incoming protobuf message is valid.
- There are cases where validation is not strict enough, leading to invalid inputs getting through to the protocol.

## Smart Contract Audits 

#### Halborn 04 - ZRC20 Lacks Resistance to ERC20 Race Condition Issue 
- There is a well known [frontrunning problem](https://ethereum.stackexchange.com/questions/93717/how-can-we-stop-front-running-for-approve) within the ``approve()`` function for the ERC20 standard.
- The ZRC20 specification is vulnerable to this.
- This was solved by implementing the ``increaseAllowance`` and ``decreaseAllowance`` as well.

#### Peckshield 1/Quantum Brief 1/Halborn 01/Halborn 02: Accommodation of Non-ERC20-Compliant Tokens (low)
- Any ERC20 token is supported. Since some of these are deflationary tokens or non-compliant tokens, issues can occur.
- Use ``safe`` variants of functions to prevent issues.

#### Peckshield 2: Trust Issue of Admin Keys (medium) - invalid
- The ``tssAddress`` allows for the updating of important values cross chain, such as the zeta supply and more.
- The authors claim this to be a *centralization risk*.
- However, the TSS address is a decentralized address with all of the validators owning a small bit of the key. So, I don't believe this finding is valid.

#### Verdise 1/2: Possible Event Reordering via Reentrancy in getEthFromZeta/getZetaFromToken
- There are two reentrance sinks within the ``getEthFromZeta()`` and ``getZetaFromToken()`` functions via a standard Solidity fallback or a malicious ERC20 token.
- This could allow for the *reordering* of events within the system, since the event is emitted *after* the call. This violates the CEI principle.
- The impact of this on the wider ecosystem is unknown though.

#### Halborn 05: Insecure Use of Tx.Origin in the Send Function 
- When emitting an event, the ``tx.origin`` is used for the *receiver* of the tokens cross chain.
- ``msg.sender`` is the preferred way to do this, since ``tx.origin`` is vulnerable to phishing attacks.
- This issue was not fixed since a user would need to do this to themselves. 

#### Halborn 09: Missing slippage/min-return check on crosschain call function
- ``minAmountOut`` is a way to prevent a user from getting very few tokens for what they paid for.
- The act of the price changing from underneath the user is called *slippage*.
- The contracts do not have slippage protections when going cross chain.
- An attacker may *sandwich* attack a user to get a profit.
- This issue was not resolved.

## Denial of Service Testing
#### Halborn 01: DoS By Issuing Large Number of Cross Chain Swaps (critical)
- Sending 10-25 transactions at once from EVM compatiable chains caused issues within the network.
- The root cause of this was never found by the auditors. But, there were many *pending* transactions in the queue for a long time.


#### Halborn 02: Loss of Funds Due to Incorret Gas Price Listing (critical)
- To transfer assets across networks a user deposits a balance of the supported token to the ``connector`` contract.
- The node will read the deposti and issue the corresponding asset.
- On top of this, a user must deposit enough to cover the gas costs.
- However, the *expected* gas cost is not the same between the connector and the RPC API.
- If the deposit does not reach the expected gas price, the users funds are lost.
- The solution was to make the different locations consistent on the pricing to prevent this from occurring.

#### Halborn 03: Loss of Funds due to illiquid uniswap gas pools (medium)
- To transfer assets across the Zetachain network, a user deposits a balance of the supported token into the connector. This will then deposit a wrapped version of this.
- The asset is paired with a UniswapV2 pool, which determines the price of the token.
- If the pool lacks liquidity, then ``getAmountsIn``` can revert. This results in users not being able to send funds cross chain.

#### Halborn 04: Possible Network Congestion due to ecosystem incentives (low)
- There is no transaction ordering in the default version of Cosmos.
- So, when big events are about to occur, network spamming is performed to make sure that a transaction gets through.
- This results in network congestion, especially on other chains like Osmosis and Juno.
- The recommended fix was to use ``mev-tendermint`` to allow for transaction ordering by tipping. This was not implemented though.

- Resources for this: 
    - Zellic Node: https://drive.google.com/file/d/1TjLkNn9MobjGTupukJBxnpxr0DKvj_6V/view
    - Halborn Node: https://drive.google.com/file/d/1323iwH34kOqGzBZIz4iX-Qfo8ACzomNc/view
    - Peckshield Smart Contract: https://drive.google.com/file/d/1cuWzLPjobQGafYLaVPZAHvyXTMOo0zbw/view

