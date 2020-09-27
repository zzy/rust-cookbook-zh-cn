## 验证正切（tan）等于正弦（sin）除以余弦（cos）

<!--
> [science/mathematics/trigonometry/tan-sin-cos.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/trigonometry/tan-sin-cos.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std] [![cat-science-badge]][cat-science]

验证 tan(x) 是否等于 sin(x)/cos(x)，其中 x=6。

```rust,edition2018
fn main() {
    let x: f64 = 6.0;

    let a = x.tan();
    let b = x.sin() / x.cos();

    assert_eq!(a, b);
}
```
