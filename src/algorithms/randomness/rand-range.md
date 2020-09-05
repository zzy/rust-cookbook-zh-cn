## 生成范围内随机数

<!--
> [algorithms/randomness/rand-range.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/algorithms/randomness/rand-range.md)
> <br />
> commit - 203b1085212a7b857d9a29bdc6a763515e77e0f9 - 2020.06.08
-->

[![rand-badge]][rand] [![cat-science-badge]][cat-science]

使用 [`Rng::gen_range`]，在半开放的 `[0, 10)` 范围内（不包括 `10`）生成一个随机值。

```rust,edition2018
use rand::Rng;

fn main() {
    let mut rng = rand::thread_rng();
    println!("Integer: {}", rng.gen_range(0, 10));
    println!("Float: {}", rng.gen_range(0.0, 10.0));
}
```

使用 [`Uniform`] 模块可以得到[均匀分布][uniform distribution]的值。下述代码和上述代码具有相同的效果，但在相同范围内重复生成数字时，下述代码性能可能会更好。

```rust,edition2018

use rand::distributions::{Distribution, Uniform};

fn main() {
    let mut rng = rand::thread_rng();
    let die = Uniform::from(1..7);

    loop {
        let throw = die.sample(&mut rng);
        println!("Roll the die: {}", throw);
        if throw == 6 {
            break;
        }
    }
}
```

[`Uniform`]: https://docs.rs/rand/*/rand/distributions/uniform/struct.Uniform.html
[`Rng::gen_range`]: https://doc.rust-lang.org/rand/*/rand/trait.Rng.html#method.gen_range
[uniform distribution]: https://en.wikipedia.org/wiki/Uniform_distribution_(continuous)
