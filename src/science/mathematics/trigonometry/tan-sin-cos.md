## 验证正切（tan）等于正弦（sin）除以余弦（cos）

[![std-badge]][std] [![cat-science-badge]][cat-science]

Verifies tan(x) is equal to sin(x)/cos(x) for x = 6.

```rust,edition2018
fn main() {
    let x: f64 = 6.0;

    let a = x.tan();
    let b = x.sin() / x.cos();

    assert_eq!(a, b);
}
```
