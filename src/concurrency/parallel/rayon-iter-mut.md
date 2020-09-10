## 并行改变数组中元素

<!--
> [concurrency/parallel/rayon-iter-mut.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/concurrency/parallel/rayon-iter-mut.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![rayon-badge]][rayon] [![cat-concurrency-badge]][cat-concurrency]

下面的实例使用了 `rayon` crate，这是一个 Rust 程序设计语言的数据并行库。`rayon` 为任何并行可迭代的数据类型提供 [`par_iter_mut`] 方法。这是一个类迭代器的链，可以对链内的数据并行计算。

```rust,edition2018
use rayon::prelude::*;

fn main() {
    let mut arr = [0, 7, 9, 11];
    arr.par_iter_mut().for_each(|p| *p -= 1);
    println!("{:?}", arr);
}
```

[`par_iter_mut`]: https://docs.rs/rayon/*/rayon/iter/trait.IntoParallelRefMutIterator.html#tymethod.par_iter_mut
