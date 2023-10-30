sequenceDiagram
    participant Observers as Observers
    participant Voting as Voting Process
    participant InboundTx as Inbound Transaction Handling
    participant OutboundTx as Outbound Transaction Processing
    participant Permissions as Permissions
    participant State as State Management
    
    Observers->>+Voting: Vote on observed transactions
    Voting->>+InboundTx: Handle Inbound CCTXs
    InboundTx->>+OutboundTx: Mark transaction "pending outbound" (if destination is not ZetaChain)
    OutboundTx->>Observers: Watch for pending outbound transactions on ZetaChain
    Observers->>OutboundTx: Participate in TSS keysign ceremony
    OutboundTx->>Observers: Broadcast signed transaction to connected blockchains
    Observers->>OutboundTx: Monitor observed outbound transactions on connected chains
    OutboundTx->>Voting: Vote on confirmed outbound transactions in ZetaChain
    Voting->>State: Update state based on finalized transactions
    Permissions->>Observers: Grant permissions for voting, creating TSS voters, etc.
    State->>Observers: Store and manage state, including transactions, TSS key, and gas prices
