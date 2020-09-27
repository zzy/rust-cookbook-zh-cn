## Vector 比较

<!--
> [science/mathematics/linear_algebra/vector-comparison.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/linear_algebra/vector-comparison.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![ndarray-badge]][ndarray]

[ndarray] crate 支持多种创建数组的方法——此实例使用 `from` 从 `std::Vec` 创建数组 [`ndarray::Array`]。然后，对数组以元素方式求和。

下面的实例按元素方式比较两个浮点型 vector。浮点数的存储通常不精确，因此很难进行精确的比较。但是，[`approx`] crate 中的 [`assert_abs_diff_eq!`] 宏允许方便地比较浮点型元素。要将 `approx` 和 `ndarray` 两个 crate一起使用，必须在 `Cargo.toml` 文件中的 `ndarray` 依赖项添加 `approx` 特性。例如：`ndarray = { version = "0.13", features = ["approx"] }`。

此实例还包含其他所有权示例。在这里，`let z = a + b` 执行后，会销毁 `a` and `b`，然后所有权会转移到 `z`。或者，`let w = &c + &d` 创建一个新的 vector，而不销毁 `c` 或者 `d`，允许以后对它们进行修改。有关其他详细信息，请参见[带有两个数组的二进制运算符][Binary Operators With Two Arrays]。

```rust,edition2018
use approx::assert_abs_diff_eq;
use ndarray::Array;

fn main() {
  let a = Array::from(vec![1., 2., 3., 4., 5.]);
  let b = Array::from(vec![5., 4., 3., 2., 1.]);
  let mut c = Array::from(vec![1., 2., 3., 4., 5.]);
  let mut d = Array::from(vec![5., 4., 3., 2., 1.]);

  let z = a + b;
  let w =  &c + &d;

  assert_abs_diff_eq!(z, Array::from(vec![6., 6., 6., 6., 6.]));

  println!("c = {}", c);
  c[0] = 10.;
  d[1] = 10.;

  assert_abs_diff_eq!(w, Array::from(vec![6., 6., 6., 6., 6.]));

}
```

[`approx`]: https://docs.rs/approx/*/approx/index.html
[`assert_abs_diff_eq!`]: https://docs.rs/approx/*/approx/macro.assert_abs_diff_eq.html
[Binary Operators With Two Arrays]: https://docs.rs/ndarray/*/ndarray/struct.ArrayBase.html#binary-operators-with-two-arrays
[ndarray]: https://docs.rs/crate/ndarray/*
[`ndarray::Array`]: https://docs.rs/ndarray/*/ndarray/struct.ArrayBase.html
