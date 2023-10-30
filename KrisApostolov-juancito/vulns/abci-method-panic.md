# ABCI Method Panic

The Go `panic` control flow function interrupts the regular execution of the code, causing it to enter a state of panic. If such a panic occurs within an [ABCI](https://github.com/tendermint/tendermint/blob/v0.34.x/spec/abci/abci.md) chain method, it will result in the suspension of chain block production and a complete halt of the chain's operation.

## Examples

The following is a simple example illustrating what entering a panic state may resemble:

```go
// ABCI Lifecycle method:
func BeginBlock(ctx sdk.Context, k keeper.Keeper) {
    someConditionValidator(ctx, k) // Will make the whole chain's execution enter panic mode <
    // ...
}

func someConditionValidator(ctx sdk.Context, k keeper.Keeper) {
	// Simple pseudo condition checker:
	if !k.condition {
			panic("Invalid condition")
	}
}
```

- [Vulnerability Report: GO-2023-1881](https://pkg.go.dev/vuln/GO-2023-1881)
- [BEP-255](https://www.binance.com/en/feed/post/783345)
- [Gravity Bridge can `panic` in multiple locations in the `EndBlocker` method](https://giters.com/althea-net/cosmos-gravity-bridge/issues/348)
- [Agoric `panic`s purposefully if the `PushAction` method returns an error](https://github.com/Agoric/agoric-sdk/blob/9116ede69169ebb252faf069d90022e8e05c6a4e/golang/cosmos/x/vbank/module.go#L166)
- [Setting invalid parameters in `x/distribution` module causes `panic` in `BeginBlocker`](https://github.com/cosmos/cosmos-sdk/issues/5808). Valid parameters are [described in the documentation](https://docs.cosmos.network/v0.45/modules/distribution/07_params.html)
- [ZetaNode Halborn Security Assessment: (HAL-03) POSSIBLE DIVISION BY ZERO COULD CAUSE CHAIN HALT DUE TO PANIC](https://drive.google.com/file/d/1323iwH34kOqGzBZIz4iX-Qfo8ACzomNc/view)
- [ZetaChain Zellic Audit Report: 3.10 No panic handler in Zetaclient may halt cross-chain communication](https://drive.google.com/file/d/1TjLkNn9MobjGTupukJBxnpxr0DKvj_6V/view)
