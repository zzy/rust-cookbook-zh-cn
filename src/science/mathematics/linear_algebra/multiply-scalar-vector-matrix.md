## 标量、vector、矩阵相乘

<!--
> [science/mathematics/linear_algebra/multiply-scalar-vector-matrix.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/linear_algebra/multiply-scalar-vector-matrix.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![ndarray-badge]][ndarray] [![cat-science-badge]][cat-science]

使用 [`ndarray::arr1`] 创建一维（1-D）数组（vector），使用 [`ndarray::arr2`] 创建二维（2-D）数组（矩阵）。

首先，一个标量乘以一个 vector 得到另一个 vector。然后，使用 [`ndarray::Array2::dot`] 将矩阵乘以新的 vector（矩阵相乘使用 `dot` 函数，而 `*` 运算符执行元素方式的乘法）。

在 `ndarray` crate 中，根据上下文，一维数组可以解释为行 vector 或列 vector。如果 vector 表示的方向很重要，则必须使用只有一行或一列的二维（2-D）数组。在本实例中，vector 是右侧的一维（1-D）数组，因此 `dot` 函数将其处理为列 vector。

```rust,edition2018
use ndarray::{arr1, arr2, Array1};

fn main() {
    let scalar = 4;

    let vector = arr1(&[1, 2, 3]);

    let matrix = arr2(&[[4, 5, 6],
                        [7, 8, 9]]);

    let new_vector: Array1<_> = scalar * vector;
    println!("{}", new_vector);

    let new_matrix = matrix.dot(&new_vector);
    println!("{}", new_matrix);
}
```

[`ndarray::arr1`]: https://docs.rs/ndarray/*/ndarray/fn.arr1.html
[`ndarray::arr2`]: https://docs.rs/ndarray/*/ndarray/fn.arr2.html
[`ndarray::Array2::dot`]: https://docs.rs/ndarray/*/ndarray/struct.ArrayBase.html#method.dot-1