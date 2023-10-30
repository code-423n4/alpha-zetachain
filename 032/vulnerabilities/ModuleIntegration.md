- [Module Integration](#module-integration)
  - [Staking Module](#staking-module)
  - [Banking Module](#banking-module)
  - [Crisis Module](#crisis-module)
  - [Governance Module](#governance-module)

### Module Integration 
- The Cosmos SDK is made up of bite-sized components. From the ``banking`` module to ``staking`` to external ones like ``Ethermint``. Being able to understand the functions and assumptions of these from a security perspective is incredibly important. 
- A few examples of integration issues are listed below. But, this is not a comprehensive list. 
- List of Cosmos SDK base modules: https://docs.cosmos.network/main/build/modules/

#### Staking Module 
- The ``staking`` module provides the proof of stake system on Cosmos. This allows users to become validators, stake with other validators and more. 
    - https://docs.cosmos.network/v0.46/modules/staking/
- This holds much of the information about the voting process itself, such as the total validator power and the *weights* of each validator. 
- If the validator count is solely used then a sybil attack could occur. This is because the voting system is weighted based upon how many tokens are staked.
- If used for checking for access control, ensure that it's a *bonded* validator and not simply a validator. 
- interface: https://github.com/cosmos/cosmos-sdk/blob/7ad7f47c892800b359d768bb99158cf6388bf115/x/staking/keeper/keeper.go#L45


#### Banking Module
- The ``banking`` module keeps track of user tokens within the Cosmos ecosystem. It has built in functionality for transferring and delegating funds. 
    - https://docs.cosmos.network/main/build/modules/bank 
- There is a bunch of built-in security protections when using the banking module. For instance, integer overflows within the ``Coins`` module will revert instead of corrupt. 
- Token metadata should be proper. For instance, the name and symbol should be verified. 
- For various math operations with tokens, the *decimals* between two tokens should be taken into consideration. 
- Calling ``MintCoins`` and ``BurnCoins`` arbitrarily could cause major havoc. 
- Blacklisted addresses within the banking module should not be interacted with.
- Keeper interface: 
    - https://docs.cosmos.network/main/build/modules/bank#basekeeper

#### Crisis Module 
- Developers are able to put in ``invariants`` that can be tested. If one of these invariants fails the check, then the blockchain will be shut down so that it can be fixed. 
    - https://docs.cosmos.network/main/build/modules/crisis
- The crisis module call calls a ``panic()`` within a message, meaning that the panic handler will work. So, the module actually does not crash the blockchain as one would expect. 
- As a result, relying on this module in case of corruption is not a good idea. 
- https://github.com/cosmos/cosmos-sdk/security/advisories/GHSA-qfc5-6r3j-jj22

#### Governance Module 
- The Cosmos SDK has a built in on-chain goverance system. There are submission, voting and penalties. 
    - https://docs.cosmos.network/main/build/modules/gov
- This can be used to call arbitrary messages as the privileged governance entity. Of course, it can call any module with special permissions, such as ``Ethermint``. It can also be used to update chain parameters, the code for the node itself and more. 
- The ``gov`` module makes it possible to integrate custom functionality upon a proposal passing as well. 
- Proper input validation on custom gov handlers should still happen. Otherwise, users may collude to break the system in some way. 
- On sensitive calls, ensure that the ``gov`` user is the only one who can call the functionality. 
