## 使用给定断言并行搜索项

<!--
> [concurrency/parallel/rayon-parallel-search.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/concurrency/parallel/rayon-parallel-search.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![rayon-badge]][rayon] [![cat-concurrency-badge]][cat-concurrency]

下面的实例使用 [`rayon::find_any`] 和 [`par_iter`] 并行搜索 vector 集合，以查找满足指定闭包中的断言的元素。

如果有多个元素满足 [`rayon::find_any`] 闭包参数中定义的断言，`rayon` 将返回搜索发现的第一个元素，但不一定是 vector 集合的第一个元素。 

请注意，实例中闭包的参数是对引用的引用（`&&x`）。有关更多详细信息，请参阅关于 [`std::find`] 的讨论。

```rust,edition2018
use rayon::prelude::*;

fn main() {
    let v = vec![6, 2, 1, 9, 3, 8, 11];

    let f1 = v.par_iter().find_any(|&&x| x == 9);
    let f2 = v.par_iter().find_any(|&&x| x % 2 == 0 && x > 6);
    let f3 = v.par_iter().find_any(|&&x| x > 8);

    assert_eq!(f1, Some(&9));
    assert_eq!(f2, Some(&8));
    assert!(f3 > Some(&8));
}
```

[`par_iter`]: https://docs.rs/rayon/*/rayon/iter/trait.IntoParallelRefIterator.html#tymethod.par_iter
[`rayon::find_any`]: https://docs.rs/rayon/*/rayon/iter/trait.ParallelIterator.html#method.find_any
[`std::find`]: https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.find
