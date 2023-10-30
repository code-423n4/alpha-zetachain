- [Bridge Integration](#bridge-integration)
  - [Timeouts](#timeouts)
  - [Relayer](#relayer)
  - [Supply Growth](#supply-growth)
  - [Input Validation](#input-validation)
  - [Event Handling](#event-handling)
  - [Voting](#voting)

### Bridge Integration 
- Cosmos SDK is a world of application blockchains. To go between these, a protocol called Inter-blockchain communication (IBC) is used. This is not used by Zetachain though, since we're going between native tokens and EVM chains. 
- Most bridges function in a specific way: 
    - On chain A, *store* the native asset trying to be transferred. On chain B, *mint* a representation of the asset from chain A. 
    - On chain B, *burn* the wrapped asset trying to be transferred. On chain A, *send* the native asset back to the user. 
- There are many edge cases to handle here that make it complicated. 

#### Timeouts 
- What happens if a call *fails*? Most bridges have functionality to *refund* the user in the case of this happening. 
- When the *error* detection is bad, then it can lead to double spend vulnerabilities. 
- If a timeout occurs on a transaction, can the *original* transaction allowed to be sent again after the timeout? 
- Is it possible to make a transaction look like it failed when it really didn't? 
- https://jumpcrypto.com/writing/huckleberry-ibc-event-hallucinations/
- https://github.com/Stride-Labs/stride/pull/818

#### Relayer
- The relayer is the user who sends events from one blockchain to another. In the context of Zetachain, the 'observer' plays this role but also has the capability to *sign* outgoing transactions and *vote* on incoming ones. 
- When the relayer has a vulnerability in it, then this can be just as bad as the real blockchain. We can trick the client into thinking something occurred when it really did not.
- If the relayer can be knocked offline by a crash, resource exhaustion or another issue, then the bridge will come to a complete halt. Error handling to prevent crashing is important here.
- Trick the relayer to *sign* transactions that it shouldn't. In Zetachain, a user sending ZETA to the bitcoin network assumed it was Bitcoin and sent the user Bitcoin instead. 
- Trick the relayer to include or drop events that occurred. 

#### Supply Growth 
- Using the lock+mint and burn+send method is great. However, it's important to keep the assets 1 to 1 between the blockchain. 
- *Inflation* of the wrapped token on the chain would result in too many funds being sent to the user. 
- *Deflation* would result in funds be impossible to withdraw. 

#### Input Validation 
- Coming up with an untamperable and source of truth between both blockchains is difficult. Rigoriously checking the inputs is crucial. 
- If a user sends a token with the same name and same symbol as a token to a different chain, it should not be valid. It should be based upon the address. 
    - https://twitter.com/PlasmaPower0/status/1582176532985880576
- Every input being provided by the user should not verified to ensure there is no trickery occurring. 
- https://rekt.news/thorchain-rekt/
- https://medium.com/immunefi/aurora-improper-input-sanitization-bugfix-review-a9376dac046f

#### Event Handling
- How do we know if an event occurred or not? In most ecosystems, *events* are watched from the blockchain attempting to relay from. 
- In Zetachain, one of the early findings from Zellic was that the event being emitted from the zEVM was not checking a particular *sender*. This resulted in an arbitrary contract being able to emit events. Other confused deputy problems may be possible as well.
    - https://www.halborn.com/blog/post/explained-the-qubit-hack-january-2022
- In Zetachain, another finding was that the same event could be observed twice, resulting in double the funds being sent. 
- Spoofing, resending, arbitrary dropping are all issues that occur that should be considered. 
- Block reorganizations can occur. To prevent double spends with this, it's important to ensure that enough blockchain confirmations have occurred to make this impossible. 
- https://www.halborn.com/blog/post/explained-the-thorchain-hack-july-2021 

#### Voting 
- When determining if an event occurred, this can be done via proofs (cryptography) or voting. In the case of Zetachain, it's done via voting. 
- Voting can have many issues that lead to unfair practices. 
- Ensure that the *weights* are taken into account. 
- Ensure that users cannot vote twice. An example of this in the Celer network is linked below: 
    - https://jumpcrypto.com/writing/election-fraud-double-voting-in-celers-state-guardian-network/
- No malleable keys for duplicate checks occur. This occured on Zetachain already. 
- Sybil attacks - also known as multiple identity attacks
- Other voting issues: 
    - https://github.com/yearn/yearn-security/blob/master/disclosures/2022-11-01.md
    - https://medium.com/@blockian/striking-gold-at-30-000-feet-uncovering-a-critical-vulnerability-in-q-blockchain-for-50-000-ab335042147b


- List of other bridge hacking resources: 
    - https://medium.com/immunefi/common-cross-chain-bridge-vulnerabilities-d8c161ffaf8f
    - https://www.certik.com/resources/blog/GuBAYoHdhrS1mK9Nyfyto-cross-chain-vulnerabilities-and-bridge-exploits-in-2022
