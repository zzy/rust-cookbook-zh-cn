## 并行测试集合中任意或所有的元素是否匹配给定断言

<!--
> [concurrency/parallel/rayon-any-all.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/concurrency/parallel/rayon-any-all.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![rayon-badge]][rayon] [![cat-concurrency-badge]][cat-concurrency]

这个实例示范如何使用 [`rayon::any`] 和 [`rayon::all`] 方法，这两个方法是分别与 [`std::any`] 和 [`std::all`] 相对应的并行方法。[`rayon::any`] 并行检查迭代器的任意元素是否与断言匹配，并在找到一个匹配的元素时就返回。[`rayon::all`] 并行检查迭代器的所有元素是否与断言匹配，并在找到不匹配的元素时立即返回。

```rust,edition2018
use rayon::prelude::*;

fn main() {
    let mut vec = vec![2, 4, 6, 8];

    assert!(!vec.par_iter().any(|n| (*n % 2) != 0));
    assert!(vec.par_iter().all(|n| (*n % 2) == 0));
    assert!(!vec.par_iter().any(|n| *n > 8 ));
    assert!(vec.par_iter().all(|n| *n <= 8 ));

    vec.push(9);

    assert!(vec.par_iter().any(|n| (*n % 2) != 0));
    assert!(!vec.par_iter().all(|n| (*n % 2) == 0));
    assert!(vec.par_iter().any(|n| *n > 8 ));
    assert!(!vec.par_iter().all(|n| *n <= 8 )); 
}
```

[`rayon::all`]: https://docs.rs/rayon/*/rayon/iter/trait.ParallelIterator.html#method.all
[`rayon::any`]: https://docs.rs/rayon/*/rayon/iter/trait.ParallelIterator.html#method.any
[`std::all`]: https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.all
[`std::any`]: https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.any
