## （反）序列化矩阵

<!--
> [science/mathematics/linear_algebra/deserialize-matrix.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/linear_algebra/deserialize-matrix.md)
> <br />
> commit 019c0a0d2cb2ffde562da4c466d090f5854c7995 - 2020.06.14
-->

[![ndarray-badge]][ndarray] [![cat-science-badge]][cat-science]

本实例实现将矩阵序列化为 JSON，以及从 JSON 反序列化出矩阵。序列化由 [`serde_json::to_string`] 处理，[`serde_json::from_str`] 则执行反序列化。

请注意：序列化后再反序列化将返回原始矩阵。

```rust
extern crate nalgebra;
extern crate serde_json;

use nalgebra::DMatrix;

fn main() -> Result<(), std::io::Error> {
    let row_slice: Vec<i32> = (1..5001).collect();
    let matrix = DMatrix::from_row_slice(50, 100, &row_slice);

    // 序列化矩阵
    let serialized_matrix = serde_json::to_string(&matrix)?;

    // 反序列化出矩阵
    let deserialized_matrix: DMatrix<i32> = serde_json::from_str(&serialized_matrix)?;

    // 验证反序列化出的矩阵 `deserialized_matrix` 等同于原始矩阵 `matrix`
    assert!(deserialized_matrix == matrix);

    Ok(())
}
```

[`serde_json::to_string`]: https://docs.rs/serde_json/*/serde_json/fn.to_string.html
[`serde_json::from_str`]: https://docs.rs/serde_json/*/serde_json/fn.from_str.html
