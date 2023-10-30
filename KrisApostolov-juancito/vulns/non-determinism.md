# Non-Determinism

Any form of non-determinism in consensus-related code can lead to vulnerabilities in the blockchain, as different validators may not agree on the transaction outcome, potentially causing a chain split.

The following non-deterministic practices can cause such issues in consensus-related code:

- [Iterations Over Unordered Structures](https://go.dev/blog/maps#iteration-order)
- [goroutines and **`select`** statement](https://github.com/golang/go/issues/33702)
- [Arbitrary Memory Addresses](https://github.com/cosmos/cosmos-sdk/issues/11726#issuecomment-1108427164)
- [Floating Point Arithmetic Operations](https://en.wikipedia.org/wiki/Floating-point_arithmetic#Accuracy_problems)
- [Randomness (Non-deterministic by design)](https://github.com/golang/go/issues/42701)
- Local time and timezones
- [Implementation (platform) dependent types like **`int`**](https://go.dev/ref/spec#Numeric_types)

## **Examples**

- [Cyber's had problems with **`float64`** type](https://github.com/cybercongress/go-cyber/issues/66)
- [ThorChain halt due to "iteration over a map error-ing at different indexes"](https://gitlab.com/thorchain/thornode/-/issues/1169)
- [ZetaNode Halborn Security Assessment: (HAL-08) ITERATION OVER MAPS MAY BE A SOURCE OF NON-DETERMINISM](https://drive.google.com/file/d/1323iwH34kOqGzBZIz4iX-Qfo8ACzomNc/view)
