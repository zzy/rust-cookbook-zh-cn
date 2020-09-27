## 矩阵相加

<!--
> [science/mathematics/linear_algebra/add-matrices.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/linear_algebra/add-matrices.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![ndarray-badge]][ndarray] [![cat-science-badge]][cat-science]

使用 [`ndarray::arr2`] 创建两个二维（2-D）矩阵，并按元素方式求和。

注意：sum 的计算方式为 `let sum = &a + &b`，借用 `&` 运算符获得 `a` 和 `b` 的引用，可避免销毁他们，使它们可以稍后显示。这样，就创建了一个包含其和的新数组。

```rust,edition2018
use ndarray::arr2;

fn main() {
    let a = arr2(&[[1, 2, 3],
                   [4, 5, 6]]);

    let b = arr2(&[[6, 5, 4],
                   [3, 2, 1]]);

    let sum = &a + &b;

    println!("{}", a);
    println!("+");
    println!("{}", b);
    println!("=");
    println!("{}", sum);
}
```

[`ndarray::arr2`]: https://docs.rs/ndarray/*/ndarray/fn.arr2.html
