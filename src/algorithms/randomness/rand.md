## 生成随机数

<!--
> [algorithms/randomness/rand.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/algorithms/randomness/rand.md)
> <br />
> commit - c6d6b58835980bd307a5974c6742cedf3cca022c - 2018.09.20
-->

[![rand-badge]][rand] [![cat-science-badge]][cat-science]

在随机数生成器 [`rand::Rng`] 的帮助下，通过 [`rand::thread_rng`] 生成随机数。可以开启多个线程，每个线程都有一个初始化的生成器。整数在其类型范围内均匀分布，浮点数是从 0 均匀分布到 1，但不包括 1。

```rust,edition2018
use rand::Rng;

fn main() {
    let mut rng = rand::thread_rng();

    let n1: u8 = rng.gen();
    let n2: u16 = rng.gen();
    println!("Random u8: {}", n1);
    println!("Random u16: {}", n2);
    println!("Random u32: {}", rng.gen::<u32>());
    println!("Random i32: {}", rng.gen::<i32>());
    println!("Random float: {}", rng.gen::<f64>());
}
```

[`rand::Rng`]: https://docs.rs/rand/*/rand/trait.Rng.html
[`rand::thread_rng`]: https://docs.rs/rand/*/rand/fn.thread_rng.html
