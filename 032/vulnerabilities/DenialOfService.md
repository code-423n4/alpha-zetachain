- [Denial of Service Vulnerabilities](#denial-of-service-vulnerabilities)
  - [Divide by 0](#divide-by-0)
  - [Resource exhaustion](#resource-exhaustion)
  - [Non-deterimism](#non-deterimism)


### Denial of Service Vulnerabilities 
- Crashes of the blockchain are awful, since no one can use the blockchain. In Ethereum, the infrastructure is solid from years of use and auditing. With Cosmos-based blockchains, vulnerabilities may be discovered that take down the entire blockchain.
- It should be noted that only crashes within the ``BeginBlocker`` and ``EndBlocker`` result in the blockchain being taken down. Panics within the ``checkTx`` and ``runTx`` have a Go panic handler. 

#### Divide by 0 
- Division by 0 is undefined, leading to crashes in most cases. 
- Halborn mentions this in their report of Zetachain with specific parameters being set to zero. 
- Osmosis ``x/gamm`` module example: 
    - When Osmosis transfers all of the funds out of the pool, the amount of tokens in the pool after is zero. 
    - A division is attempted to be performed, resulting in a panic. `` y := tokenBalanceFixedBefore.Quo(tokenBalanceFixedAfter) ``
    - https://github.com/osmosis-labs/osmosis/issues/3831

#### Resource exhaustion
- In web2, many denial of service issues simply come from eating up too many resources on the machine, such as the recently discovered [rapid reset](https://blog.cloudflare.com/technical-breakdown-http2-rapid-reset-ddos-attack/) bug found in HTTP2. 
- A good example is a recent bug in Osmosis, there is a library being used for performing exponentiation. Instead of simply doing a ** b it is doing an approximation technique using a series. 
- However, there is no bound on how many interations can occur. The author of the post was able to make the iteration last for 0.8 seconds on each call to it via 2M iterations. This leads to a resource exhaustion because of the blockchain did not have the time necessary to run through the other transactions.
- https://blog.trailofbits.com/2023/10/23/numbers-turned-weapons-dos-in-osmosis-math-library/ 
- https://forum.cosmos.network/t/vulnerability-coordination-retrospective-cosmos-mainnet-security-advisory-lavender-2020-04-09/3550
- https://medium.com/immunefi/sui-temporary-total-network-shutdown-bugfix-review-c271d0319dcc

#### Non-deterimism 
- Consensus is the agreement of a state in a blockchain. It is important for the good being ran within a blockchain to be *deterministic* so that the same point can always be reached. 
- There are various points of non-determinism that can cause problems...
- **Iterating over Maps**: 
    - In Go, iterating over maps is known to be different on different versions of Go. 
    - Additionally, in newer versions, this is [intentionally randomized](https://stackoverflow.com/questions/9619479/go-what-determines-the-iteration-order-for-map-keys). 
    - Since the states may be different depending on which item was accessed first, this leads to consensus issues. 
- **Floats**: 
    - Floats have different amounts of precision on different platforms. 
    - Since the states may be different depending on the implementation used, this can cause major problems in consensus. 
    - https://github.com/cybercongress/go-cyber/issues/66
- **Time**: 
    - The execution of code may occur slightly different time windows on different nodes.
    - Additionally, the clocks on some nodes may differ slightly compared to others. 
    - Direct comparisons on time can cause major discrepancies as a result. 
    - https://forum.cosmos.network/t/cosmos-sdk-vulnerability-retrospective-security-advisory-jackfruit-october-12-2021/5349
