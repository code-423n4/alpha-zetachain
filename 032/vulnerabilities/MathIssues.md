- [Rounding](#rounding)
- [Integer Overflows/Underfllows](#integer-overflowsunderfllows)
- [Truncation](#truncation)

## Rounding 
- Similar to Solidity, rounding on token amounts and other important values needs to be taken seriously.
- ``sdk.Dec`` has problems with math operations and is commonly used throughout the Cosmos SDK.
- Performing multiplication before division is essential to keeping values proper and not losing precision.
- Ensure that the rounding (either bankers rounding up or down) benefits the module at all times.
- https://github.com/umee-network/umee/issues/545
- https://github.com/osmosis-labs/osmosis/commit/c211999848a3ae1c9ac50716949f01c3a1bb9cfe

## Integer Overflows/Underfllows
- In Golang, which is what the Cosmos SDK is written in, numbers have a fixed size.
- If the maximum or minimum size is reached then the number will *wrap*. You can think of the integers as a *clock* in this way.
- Going from the largest integer will wrap around to the smallest possible integer.
- https://www.acunetix.com/blog/web-security-zone/what-is-integer-overflow/
- https://jumpcrypto.com/writing/helping-secure-bnb-chain-through-responsible-disclosure/

## Truncation
- In most programming languages, including Golang, integers can be changed in their size.
- When type casting from one sized integer to another, the values at the top are *chopped* off.
- For instance, imagine we have a 2 byte unsigned integer (maximum value of 65535) and we convert this to a 1 byte unsigned integer (maximum value of 255). If we convert 65535 to a 1 byte unsigned integer, it will become 255 after the chopping off of the higher byte.
- https://pwning.mirror.xyz/RFNTSouIIlHVNmTNDThUVb1obIeN5c1LAiQuN9Ve-ok
