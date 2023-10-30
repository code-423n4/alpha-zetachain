### Ante Handler Issues 
- On the initial call to a Cosmos SDK blockchain, the ``pre-ante handler`` is executed. This contains code to perform basic validity checks such as signature, gas and more. 
- The default variation of this exists within the ``auth`` module. However, the ante handler of the program can be modified for various purposes, such as with Ethermint. 
    - https://docs.cosmos.network/v0.45/modules/auth/03_antehandlers.html
- The changing/removal of these should be taken very seriously.
- For instance, the removal of the ``ValidateBasicDecorator`` would cause no input validation to occur at the lower levels of the system, leading to horrible consequences. 
    - https://forum.cosmos.network/t/cosmos-sdk-ibc-vulnerability-retrospective-security-advisories-dragonberry-and-elderflower-october-2022/8735/1
- The ante handler is only executed at the *beginning* of a transaction. So, if there are nested calls in the message, message specific validation may be possible to bypass. 
- https://jumpcrypto.com/writing/bypassing-ethermint-ante-handlers/
