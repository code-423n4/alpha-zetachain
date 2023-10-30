sequenceDiagram
    participant Admin as Admin Policy Account
    participant Observer as Observer Validators
    participant Node as Node Accounts
    participant Tracker as Outbound Tx Tracker
    participant Chain as Connected Chain
    participant Zeta as ZetaChain

    Admin->>Tracker: MsgAddToOutTxTracker
    Observer->>Tracker: MsgAddToOutTxTracker
    Admin->>Tracker: MsgRemoveFromOutTxTracker
    Node->>Zeta: MsgCreateTSSVoter
    Observer->>Zeta: MsgGasPriceVoter
    Observer->>Chain: MsgVoteOnObservedOutboundTx
    Observer->>Chain: MsgVoteOnObservedInboundTx
    Admin->>Zeta: MsgWhitelistERC20
    Admin->>Zeta: MsgUpdateTssAddress

    Note right of Chain: Transactions are monitored, voted on, and processed based on the interactions between Observer Validators, the Outbound Tx Tracker, and the ZetaChain. Various messages trigger different actions, updating the state and processing transactions accordingly.
