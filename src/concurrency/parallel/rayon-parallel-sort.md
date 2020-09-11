## 对 vector 并行排序

<!--
> [concurrency/parallel/rayon-parallel-sort.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/concurrency/parallel/rayon-parallel-sort.md)
> <br />
> commit 203b1085212a7b857d9a29bdc6a763515e77e0f9 - 2020.06.08
-->

[![rayon-badge]][rayon] [![rand-badge]][rand] [![cat-concurrency-badge]][cat-concurrency]

本实例对字符串 vector 并行排序。

首先，分配空字符串 vector；然后，通过 `par_iter_mut().for_each` 并行对 vector 填充随机值。尽管存在[多种选择][multiple options]，可以对可枚举数据类型进行排序，但 [`par_sort_unstable`] 通常比[稳定排序（相同的值排序后相对顺序不变）][stable sorting]算法快。

```rust,edition2018

use rand::{Rng, thread_rng};
use rand::distributions::Alphanumeric;
use rayon::prelude::*;

fn main() {
  let mut vec = vec![String::new(); 100_000];
  vec.par_iter_mut().for_each(|p| {
    let mut rng = thread_rng();
    *p = (0..5).map(|_| rng.sample(&Alphanumeric)).collect()
  });
  vec.par_sort_unstable();
}
```

[`par_sort_unstable`]: https://docs.rs/rayon/*/rayon/slice/trait.ParallelSliceMut.html#method.par_sort_unstable
[multiple options]: https://docs.rs/rayon/*/rayon/slice/trait.ParallelSliceMut.html
[stable sorting]: https://docs.rs/rayon/*/rayon/slice/trait.ParallelSliceMut.html#method.par_sort
