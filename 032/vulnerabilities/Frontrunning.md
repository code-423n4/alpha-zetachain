### Frontrunning 
- The Cosmos SDK blockchain works as a FIFO buffer. As a result, there is no single chain frontrunning. However, collusion between nodes may cause this to be possible. 
- In the ecosystem of Zetachain, we are going between different blockchains, such as Binance to zEVM.
- Since an event on one chain can be observed and acted on to a different chain, frontrunning between blockchains is possible. 
- For instance, if a cross-chain call from Binance to zEVM was made, an attacker could see this in the mempool of Binance. Upon seeing this, they could react to perform operatinos on the zEVM before the observer voting process took place to add the transaction in. 
- This does not just apply to these two chains: cross-chain frontrunning is very possible within this context. 
