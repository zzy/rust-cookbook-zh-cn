## 矩阵相乘

<!--
> [science/mathematics/linear_algebra/multiply-matrices.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/linear_algebra/multiply-matrices.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![ndarray-badge]][ndarray] [![cat-science-badge]][cat-science]

使用 [`ndarray::arr2`] 创建两个矩阵，并使用 [`ndarray::ArrayBase::dot`] 对它们执行矩阵乘法。

```rust,edition2018
use ndarray::arr2;

fn main() {
    let a = arr2(&[[1, 2, 3],
                   [4, 5, 6]]);

    let b = arr2(&[[6, 3],
                   [5, 2],
                   [4, 1]]);

    println!("{}", a.dot(&b));
}
```

[`ndarray::arr2`]: https://docs.rs/ndarray/*/ndarray/fn.arr2.html
[`ndarray::ArrayBase::dot`]: https://docs.rs/ndarray/*/ndarray/struct.ArrayBase.html#method.dot-1
