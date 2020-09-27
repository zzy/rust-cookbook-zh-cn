## 创建复数

<!--
> [science/mathematics/complex_numbers/create-complex.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/complex_numbers/create-complex.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![num-badge]][num] [![cat-science-badge]][cat-science]

创建类型 [`num::complex::Complex`] 的复数，复数的实部和虚部必须是同一类型。

```rust,edition2018
fn main() {
    let complex_integer = num::complex::Complex::new(10, 20);
    let complex_float = num::complex::Complex::new(10.1, 20.1);

    println!("Complex integer: {}", complex_integer);
    println!("Complex float: {}", complex_float);
}
```

[`num::complex::Complex`]: https://autumnai.github.io/cuticula/num/complex/struct.Complex.html
