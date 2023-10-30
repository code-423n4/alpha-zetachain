# Decimal Precision Rounding Errors

The [Cosmos SDK](https://pkg.go.dev/cosmossdk.io/math) offers two custom types for dealing with numbers

- `sdk.Int` and `sdk.UInt` type for integer arithmetic.
- `sdk.Dec` type for decimal arithmetic.

`sdk.Dec` has rounding issues, which should be taken into consideration when integrating with it.

## Examples

The following PoC illustrates a rounding issue with `sdk.Dec`:

```go
import (
	"testing"
)

func TestSDKDec(t *testing.T) {
    a := sdk.MustNewDecFromStr("10")
    b := sdk.MustNewDecFromStr("1000000010")
    x := a.Quo(b).Mul(b)
    expectedX := sdk.MustNewDecFromStr("9.999999999999999000")
    if !x.Equal(expectedX) {
        t.Errorf("x is not equal to the expected value. Got: %s, Expected: %s", x.String(), expectedX.String())
    }

    q := float32(10)
    w := float32(1000000010)
    y := (q / w) * w
    expectedY := float32(10)
    if y != expectedY {
        t.Errorf("y is not equal to the expected value. Got: %f, Expected: %f", y, expectedY)
    }
}
```

- https://github.com/cosmos/cosmos-sdk/issues/7773
- https://github.com/umee-network/umee/issues/545
