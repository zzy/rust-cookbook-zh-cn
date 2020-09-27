## 计算三角形的边长

<!--
> [science/mathematics/trigonometry/side-length.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/trigonometry/side-length.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std] [![cat-science-badge]][cat-science]

计算直角三角形斜边的长度，其中斜边的角度为 2 弧度，对边长度为 80。

```rust,edition2018
fn main() {
    let angle: f64 = 2.0;
    let side_length = 80.0;

    let hypotenuse = side_length / angle.sin();

    println!("Hypotenuse: {}", hypotenuse);
}
```
