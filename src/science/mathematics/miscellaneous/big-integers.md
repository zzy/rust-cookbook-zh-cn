## 大数

<!--
> [science/mathematics/miscellaneous/big-integers.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/miscellaneous/big-integers.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![num-badge]][num] [![cat-science-badge]][cat-science]

[`BigInt`] 使得超过 128 位的大整数计算成为可能。

```rust,edition2018
use num::bigint::{BigInt, ToBigInt};

fn factorial(x: i32) -> BigInt {
    if let Some(mut factorial) = 1.to_bigint() {
        for i in 1..(x+1) {
            factorial = factorial * i;
        }
        factorial
    }
    else {
        panic!("Failed to calculate factorial!");
    }
}

fn main() {
    println!("{}! equals {}", 100, factorial(100));
}
```

[`BigInt`]: https://docs.rs/num/0.2.0/num/struct.BigInt.html