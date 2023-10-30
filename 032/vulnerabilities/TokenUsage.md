- [Token Usage](#token-usage)
  - [Bad Oracles](#bad-oracles)
  - [Denom Checks](#denom-checks)
  - [Decimals](#decimals)
  - [Improper Bookkeeping](#improper-bookkeeping)

### Token Usage
- Tokens are the main form of asset transfer in most blockchains, including the Cosmos SDK. 
- Validating that the tokens are being used correctly at both the Cosmos SDK level and EVM level are critical for security. Duplication, removal or any other issues could ruin the system. 
- A list of issues is shown below: 

#### Bad Oracles
- When trading from token X to token Y, we need to know the price of this. 
- This is commonly done with an *oracle*: an entity who decides the price for us. 
- If the oracle value itself is bad, then the entire ecosystem crumbles under this as well. 
- **Out of date oracle** can occur if the price is not updated frequently enough. An attacker could arbitrage a program if this occurs. 
- **Manipulatable**. If it's an on-chain oracle, the oracle price could be manipulated in an unfair way for the system, resulting in massive gain for the attacker. 
- https://blog.openzeppelin.com/secure-smart-contract-guidelines-the-dangers-of-price-oracles 
- https://blog.quillaudits.com/2023/07/25/decoding-palmswaps-900k-exploit/

#### Denom Checks 
- Cosmos SDK can have many native tokens all at once. 
- Properly validating that the intended token was sent is crucial. Otherwise, a less valuable asset than intended could be used. 
- https://github.com/osmosis-labs/osmosis/commit/01dee57c4abc6cf7116156cd22bb5c04afe235a1

#### Decimals
- Tokens in the Cosmos SDK also have *decimals*, like ERC20 tokens do. 
- If swaps are being performed or prices are calculated with an assumed decimal amount, problems can occur. 
- https://veefi.medium.com/the-main-cause-of-vee-finance-attack-7a8475085ec5

#### Improper Bookkeeping 
- Adding, removing and transferring funds from an account is a manual process by the programmers of a Cosmos SDK blockchain. 
- Ensuring that the assets are kept track of securely is cruical. Otherwise, an attacker could gain an inflated set of tokens. 
- In Osmosis, the *withdrawal* sent **two times** the amount of tokens that it should have: 
    - https://decrypt.co/102300/cosmos-based-defi-exchange-osmosis-hit-by-5m-exploit 
    - https://www.reddit.com/r/CryptoCurrency/comments/v7iy8t/osmosis_has_been_hacked_and_chain_halted_lps/
