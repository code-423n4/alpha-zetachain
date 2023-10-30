sequenceDiagram
    participant Block as Block Processing
    participant Emissions as Emissions Module
    participant Validators as Validators
    participant Observers as Observers
    participant TSS as TSS Signers

    Block->>+Emissions: Begin Blocker Stage
    Emissions->>+Validators: Distribute Rewards
    Emissions->>Observers: Accumulate Rewards
    Emissions->>TSS: Accumulate Rewards

    Note right of Emissions: Parameters Tracking
    Emissions->>Emissions: Track Maximum Bond Factor
    Emissions->>Emissions: Track Minimum Bond Factor
    Emissions->>Emissions: Track Average Block Time
    Emissions->>Emissions: Track Target Bond Ratio
    Emissions->>Emissions: Track Validator Emission Percentage
    Emissions->>Emissions: Track Observer Emission Percentage
    Emissions->>Emissions: Track TSS Signer Emission Percentage
    Emissions->>Emissions: Track Duration Factor Constant

    Note right of Emissions: Key Takeaways
    Emissions->>Emissions: Ensure Fair Reward Distribution
    Emissions->>Emissions: Methodical Reward Calculation
    Emissions->>Emissions: Reward Prioritization

