sequenceDiagram
    participant Admin as Admin Policy Account
    participant Store as Store
    participant Chain as Arbitrary Chain
    participant Crosschain as Crosschain Operations

    Admin->>+Store: MsgAddObserver
    Store->>Store: Add New Observer
    Store->>Store: Reset Keygen
    Store->>Store: Pause Inbound Transactions for New TSS Generation

    Admin->>+Chain: MsgUpdateCoreParams
    Chain->>Chain: Update Core Parameters
    Chain->>Admin: Error if Chain ID isn't Supported

    Admin->>+Store: MsgAddBlameVote
    Store->>Store: Add Blame Vote

    Admin->>+Crosschain: MsgUpdateCrosschainFlags
    Crosschain->>Crosschain: Update Crosschain Flags

    Admin->>+Store: MsgUpdateKeygen
    Store->>Store: Update Block Height for Key Generation
    Store->>Store: Set Status to "Pending Keygen"

    Admin->>+Store: MsgAddBlockHeader
    Store->>Store: Add Block Header based on Majority Voting by Observers
