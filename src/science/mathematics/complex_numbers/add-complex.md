## 复数相加

<!--
> [science/mathematics/complex_numbers/add-complex.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/complex_numbers/add-complex.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![num-badge]][num] [![cat-science-badge]][cat-science]

对复数执行数学运算与对内置类型执行数学运算是一样的：计算的数字必须是相同的类型（如浮点数或整数）。

```rust,edition2018
fn main() {
    let complex_num1 = num::complex::Complex::new(10.0, 20.0); // 必须为浮点数
    let complex_num2 = num::complex::Complex::new(3.1, -4.2);

    let sum = complex_num1 + complex_num2;

    println!("Sum: {}", sum);
}
```
