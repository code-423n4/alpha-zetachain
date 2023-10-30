# Lack of Error Handler

In Go, the common practice is to check errors by comparing them to nil. This method of handling errors gives the developer enough freedom to handle errors and according to the protocol's needs.

Any improper error handling implementation or lack there of can cause a big variety of vulnerabilities/unexpected behavior.

## Examples

The following is a code snippet that lacks error handling:

```go
func (k msgServer) Transfer(goCtx context.Context, msg *types.MsgTransfer) (*types.MsgTransferResponse, error) {
    err := k.bankKeeper.SendCoins(sdk.UnwrapSDKContext(goCtx), msg.Sender, msg.Receiver, msg.Amount)

    return &types.MsgTransferResponse{}, nil
}
```

Here is the altered version of the snippet, but with proper error handling:

```go
func (k msgServer) Transfer(goCtx context.Context, msg *types.MsgTransfer) (*types.MsgTransferResponse, error) {
    err := k.bankKeeper.SendCoins(sdk.UnwrapSDKContext(goCtx), msg.Sender, msg.Receiver, msg.Amount)
    if err != nil {
        // Handle the error as needed, e.g., log it or return an error response.
        return nil, err
    }

    // If no error occurred, return a success response.
    return &types.MsgTransferResponse{}, nil
}
```

- [ZetaChain Zellic Audit Report: 3.8 Missing nil check when parsing client event](https://drive.google.com/file/d/1TjLkNn9MobjGTupukJBxnpxr0DKvj_6V/view)
- [DON’T “PANIC”: HOW IMPROPER ERROR-HANDLING CAN LEAD TO BLOCKCHAIN HACKS](https://www.halborn.com/blog/post/dont-panic-how-improper-error-handling-can-lead-to-blockchain-hacks)
