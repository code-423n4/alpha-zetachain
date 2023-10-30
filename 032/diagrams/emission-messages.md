sequenceDiagram
    participant Admin as Admin Policy Account
    participant Fungible as Fungible Module
    participant ZRC20 as ZRC20 Contract
    participant System as System Contract
    participant Registry as Foreign Coins List
    participant GasPool as Gas Pool

    Admin->>+Fungible: MsgDeployFungibleCoinZRC20
    Fungible->>+ZRC20: Deploy ZRC20 Contract
    ZRC20-->>Fungible: Contract Address
    Fungible->>System: Set Contract Address
    Fungible->>Fungible: Mint ZETA to Module Account (if gas coin)
    Fungible->>GasPool: Update Gas Pool (if gas coin)
    Fungible->>Fungible: Add Liquidity (if gas coin)
    Fungible->>Registry: Add to Foreign Coins List (if non-gas coin)

    Admin->>Fungible: MsgRemoveForeignCoin
    Fungible->>Registry: Remove Coin from Foreign Coins List

    Admin->>Fungible: MsgUpdateSystemContract
    Fungible->>System: Update System Contract Address

    Fungible->>ZRC20: Update Withdrawal Fee
    Fungible->>ZRC20: Update Contract Bytecode

    Fungible->>ZRC20: Update Paused Status
