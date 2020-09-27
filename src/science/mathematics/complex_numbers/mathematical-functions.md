## 复数的数学函数

<!--
> [science/mathematics/complex_numbers/mathematical-functions.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/complex_numbers/mathematical-functions.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![num-badge]][num] [![cat-science-badge]][cat-science]

在与其他数学函数交互时，复数有一系列有趣的特性，尤其是和自然常数 e（欧拉数）类似的正弦相关函数。要将这些函数与复数一起使用，复数类型有几个内置函数，详细请参阅 [`num::complex::Complex`]。

```rust,edition2018
use std::f64::consts::PI;
use num::complex::Complex;

fn main() {
    let x = Complex::new(0.0, 2.0*PI);

    println!("e^(2i * pi) = {}", x.exp()); // =~1
}
```

[`num::complex::Complex`]: https://autumnai.github.io/cuticula/num/complex/struct.Complex.html
