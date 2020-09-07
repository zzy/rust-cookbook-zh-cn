## 生成给定分布随机数

<!--
> [algorithms/randomness/rand-dist.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/algorithms/randomness/rand-dist.md)
> <br />
> commit - 203b1085212a7b857d9a29bdc6a763515e77e0f9 - 2020.06.08
-->

[![rand_distr-badge]][rand_distr] [![cat-science-badge]][cat-science]

默认情况下，随机数在 `rand` crate 中是[均匀分布][uniform distribution]。[`rand_distr`] crate 提供其它的分布类型。如要使用，首先实例化一个分布，然后在随机数生成器 [`rand::Rng`] 的帮助下，使用 [`Distribution::sample`] 从该分布中进行采样。

关于更多信息，点击阅读[可用分布文档][rand-distributions]。如下是一个使用[`正态（Normal）`][`Normal`]分布的实例。

```rust,edition2018,ignore
use rand_distr::{Distribution, Normal, NormalError};
use rand::thread_rng;

fn main() -> Result<(), NormalError> {
    let mut rng = thread_rng();
    let normal = Normal::new(2.0, 3.0)?;
    let v = normal.sample(&mut rng);
    println!("{} is from a N(2, 9) distribution", v);
    Ok(())
}
```

[`Distribution::sample`]: https://docs.rs/rand/*/rand/distributions/trait.Distribution.html#tymethod.sample
[`Normal`]: https://docs.rs/rand_distr/*/rand_distr/struct.Normal.html
[`rand::Rng`]: https://docs.rs/rand/*/rand/trait.Rng.html
[`rand_distr`]: https://docs.rs/rand_distr/*/rand_distr/index.html
[rand-distributions]: https://docs.rs/rand_distr/*/rand_distr/index.html
[uniform distribution]: https://en.wikipedia.org/wiki/Uniform_distribution_(continuous)
