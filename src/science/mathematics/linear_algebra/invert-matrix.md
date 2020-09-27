## 矩阵求逆

<!--
> [science/mathematics/linear_algebra/invert-matrix.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/linear_algebra/invert-matrix.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![nalgebra-badge]][nalgebra] [![cat-science-badge]][cat-science]

用 [`nalgebra::Matrix3`] 创建一个 3x3 的矩阵，如果可能的话，将其求逆。

```rust,edition2018
use nalgebra::Matrix3;

fn main() {
    let m1 = Matrix3::new(2.0, 1.0, 1.0, 3.0, 2.0, 1.0, 2.0, 1.0, 2.0);
    println!("m1 = {}", m1);
    match m1.try_inverse() {
        Some(inv) => {
            println!("The inverse of m1 is: {}", inv);
        }
        None => {
            println!("m1 is not invertible!");
        }
    }
}
```

[`nalgebra::Matrix3`]: https://docs.rs/nalgebra/*/nalgebra/base/type.Matrix3.html
