## 并行改变数组中元素

<!--
> [concurrency/parallel/rayon-iter-mut.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/concurrency/parallel/rayon-iter-mut.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![rayon-badge]][rayon] [![cat-concurrency-badge]][cat-concurrency]

The example uses the `rayon` crate, which is a data parallelism library for Rust.
`rayon` provides the [`par_iter_mut`] method for any parallel iterable data type.
This is an iterator-like chain that potentially executes in parallel.

```rust,edition2018
use rayon::prelude::*;

fn main() {
    let mut arr = [0, 7, 9, 11];
    arr.par_iter_mut().for_each(|p| *p -= 1);
    println!("{:?}", arr);
}
```

[`par_iter_mut`]: https://docs.rs/rayon/*/rayon/iter/trait.IntoParallelRefMutIterator.html#tymethod.par_iter_mut
