- [Other Audit Findings](#other-audit-findings)
  - [A byzantine consumer can tombstone, slash, or jail an innocent validator - Cosmos Hub by Informal Systems](#a-byzantine-consumer-can-tombstone-slash-or-jail-an-innocent-validator---cosmos-hub-by-informal-systems)
  - [Stealing of user funds via negative deposits - Duality Informal Systems](#stealing-of-user-funds-via-negative-deposits---duality-informal-systems)
  - [Escrow Address Collisions - IBC by Informal Systems](#escrow-address-collisions---ibc-by-informal-systems)
  - [Isolating Host Zone Operations - Stride by Informal Systems](#isolating-host-zone-operations---stride-by-informal-systems)
  - [User Interface Message Spoofing - Vega Protocol by OtterSec](#user-interface-message-spoofing---vega-protocol-by-ottersec)
  - [Incorrect Tick Boundary - Osmosis by OtterSec](#incorrect-tick-boundary---osmosis-by-ottersec)
  - [Incorrect Tick Iterator Initialization - Osmosis by OtterSec](#incorrect-tick-iterator-initialization---osmosis-by-ottersec)
  - [Proposal’s tally returns wrong vote result - Neutron by Oak Security](#proposals-tally-returns-wrong-vote-result---neutron-by-oak-security)
  - [IBC Error Handling Issues - IBC by JumpCrypto](#ibc-error-handling-issues---ibc-by-jumpcrypto)

## Other Audit Findings

### A byzantine consumer can tombstone, slash, or jail an innocent validator - Cosmos Hub by Informal Systems
- Nodes can submit *evidence* that another node is mishaving. This would include double signs or downtime. 
- The evidence is *acted* on prior to verification though. 
- As a result, a malicious node can get arbitrary users slashed. 
- https://github.com/informalsystems/audits/blob/main/Cosmos%20Hub/2023-02-10%20Audit%20Report%20-%20ICS%20replicated%20security.pdf 

### Stealing of user funds via negative deposits - Duality Informal Systems
- Transactions for ``MsgDeposit`` and ``MsgWithdrawal`` take in positive or negative numbers. 
- This bug allows a user to *decrease* the amount of shares within a given pool.
- By increasing/decreasing the shares arbitrarily, it is possible to create crazy rounding situations, similar to those in the ERC 4626 inflation attack: 
    - https://blog.openzeppelin.com/a-novel-defense-against-erc4626-inflation-attacks
- In this case, the authors were able to exploit this to take funds from innocent users depositing funds into the vault.
- https://github.com/informalsystems/audits/blob/main/Duality/2023-05-16%20Audit%20Report%20-%20Duality%20Dex%20and%20Incentives%20modules.pdf

### Escrow Address Collisions - IBC by Informal Systems
- When using IBC, some address must hold tokens being sent to another chain. The address is calculated using the port ID and channel ID. In particular, ``SHA256(portID + channelID)`` is performed. This address must be unique. 
- There is no domain separation between ports and channels. ("transfer","channel") and ("trans", "ferchannel") will be the same  SHA256("transferchannel"). This results in the wrong tokens being taken.
    - https://github.com/cosmos/cosmos-sdk/issues/7737#issuecomment-726780022
- Second, hash collisions from the birthday paradox are also possible. In particular, since we are truncating to 20 bytes.
- https://github.com/informalsystems/audits/blob/main/IBC-GO/report.pdf

### Isolating Host Zone Operations - Stride by Informal Systems
- Right now, all zones are tightly coupled. If one zone fails, then all of the other zones fail as well. With a lot of code, this is a major risk. 
- Stride plans to expand to 50 host zones in the future. 
- Isolation of these zones should be performed in case issues arise. 
- IF-STRIDE-STAKEIBC-ZONEISOLATION: https://github.com/informalsystems/audits/blob/main/Stride/2022-11-30%20Audit%20Report%20-%20Stride%20StakeIBC%20ICACallbacks.pdf


### User Interface Message Spoofing - Vega Protocol by OtterSec
- ``prettyPrint`` prints a JavaScript object in the snap dialog. 
- The 'default' fallback case will print the can print transaction information with arbitrary data. 
- This could be used to spoof the transaction occuring and mislead the user in what they sign. 
- Vega: https://ottersec.notion.site/Sampled-Public-Audit-Reports-a296e98838aa4fdb8f3b192663400772


### Incorrect Tick Boundary - Osmosis by OtterSec
- The Osmosis liquidity ticks are inclusive on the lower end and not inclusive on the upper end. [lowerTick, upperTick)
- The if statement for this check was using a ``<=`` instead of a ``<``. This resulted in the bounds being *inclusive* on the upper end. 
- This has several consequences. First, a user may provide liquidity outside of the expected range, which violates slippage protections. Second, liquidity could be added to the currentTick incorrectly. 
- Osmosis: https://ottersec.notion.site/Sampled-Public-Audit-Reports-a296e98838aa4fdb8f3b192663400772

### Incorrect Tick Iterator Initialization - Osmosis by OtterSec 
- The ``KVStores`` in the Cosmos SDK are iterable. To do this, a ``prefix`` for the items to iterate over must be provided. 
- One usage of the iterator was when getting the next tick. However, the function for creating the prefix had a plus 1 in it. 
- This results in an off by 1 in when creating this iterator, which is used by swaps for calculating ranges properly. This would have resulted in pool corruption by having the wrong data being altered or viewed. 
- Osmosis: https://ottersec.notion.site/Sampled-Public-Audit-Reports-a296e98838aa4fdb8f3b192663400772

### Proposal’s tally returns wrong vote result - Neutron by Oak Security 
- The Neutron project had implemented their own goverance module instead of the standard Cosmos SDK one. 
- The voting process has yes, no and NoWithVeto. While counting up the votes, the ``NoWithVeto`` were forgotten about. 
- This results in the ``NoWithVeto`` being counted as 'yes' votes by accident. So, some votes may pass when they should not. 
- https://github.com/oak-security/audit-reports/blob/master/Neutron/2022-12-07%20Audit%20Report%20-%20Neutron%20v1.0.pdf


### IBC Error Handling Issues - IBC by JumpCrypto 
- The ``onRecvPacket`` function does not return a normal Go error message on failure. Instead, it returns an acknowledgement.
- This makes it possible to persist the acknowledgment result on-chain and relay it back to the sender chain.
- On error from the acknowledgment, no events or state change should be performed except the ack itself.
- In IBC Go, no state changes were made by ``events`` were still being emitted.
- An attacker who could find a system that trusts events could trigger an error path to force events to occur that never actually happened. 
- cBridge, Multichain and Wormhole’s legacy Cosmos integration (replaced by the IBC-native Wormhole Gateway) all depend on Cosmos events for detecting interactions.
- By calling this functionality from CosmWasm and forcibly triggering an error, it is possible to fake bridge deposits.
- https://jumpcrypto.com/writing/huckleberry-ibc-event-hallucinations/

