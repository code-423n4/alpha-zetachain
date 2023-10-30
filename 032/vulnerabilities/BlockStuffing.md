### Block Stuffing
- The Cosmos SDK blockchain works as a FIFO buffer. As a result, there is no single chain frontrunning. 
- Many of the Cosmos SDK blockchains are cheap on gas, making the congestion of the network from a single user possible. Additionally, if lots of transactions are being sent at once, then this can happen as well. This happened during product launches on Terra: 
    - https://cryptorisks.substack.com/p/ust-december-2021
- If there are sensitive actions then they should be prioritized. If not, an attacker could profit. 
- For instance, imagine there is an auction platform where the price of the asset got cheaper every block. If an attacker can profit from this, they could *stuff* the mempool to the brim with garage transactions every block until the asset is very cheap. At this point, they could submit their own transaction to claim the asset for cheap. 
- Another example is an *oracle* update. If the attacker knows that the oracle update is coming with a more expensive price, they could *stuff* the update then buy assets for each and sell them high afterwards. 
- The solution to this is to have reasonable gas prices to defend against this and to have *priority messaging*. 
- https://github.com/crytic/building-secure-contracts/tree/master/not-so-smart-contracts/cosmos/messages_priority
