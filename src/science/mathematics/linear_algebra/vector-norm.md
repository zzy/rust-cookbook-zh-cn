## Vector 范数

<!--
> [science/mathematics/linear_algebra/vector-norm.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/linear_algebra/vector-norm.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![ndarray-badge]][ndarray]

这个实例展示了 [`Array1`] 类型、[`ArrayView1`] 类型、[`fold`] 方法，以及 [`dot`] 方法在计算给定 vector 的 [l1] 和 [l2] 范数时的用法。
+ `l2_norm` 函数是两者中较简单的，它计算一个 vector 与自身的点积（dot product，数量积）的平方根。
+ `l1_norm` 函数通过 `fold` 运算来计算元素的绝对值（也可以通过 `x.mapv(f64::abs).scalar_sum()` 执行，但是会为 `mapv` 的结果分配一个新的数组）。

请注意：`l1_norm` 和 `l2_norm` 都采用 [`ArrayView1`] 类型。这个实例考虑了 vector 范数，所以范数函数只需要接受一维视图（[`ArrayView1`]）。虽然函数可以使用类型为 `&Array1<f64>` 的参数，但这将要求调用方引用拥有所有权的数组，这比访问视图更为严格（因为视图可以从任意数组或视图创建，而不仅仅是从拥有所有权的数组创建）。

`Array` 和 `ArrayView` 都是 `ArrayBase` 的类型别名。于是，大多数的调用方参数类型可以是 `&ArrayBase<S, Ix1> where S: Data`，这样调用方就可以使用 `&array` 或者 `&view` 而不是 `x.view()`。如果该函数是公共 API 的一部分，那么对于用户来说，这可能是一个更好的选择。对于内部函数，更简明的 `ArrayView1<f64>` 或许更合适。

```rust,edition2018
use ndarray::{array, Array1, ArrayView1};

fn l1_norm(x: ArrayView1<f64>) -> f64 {
    x.fold(0., |acc, elem| acc + elem.abs())
}

fn l2_norm(x: ArrayView1<f64>) -> f64 {
    x.dot(&x).sqrt()
}

fn normalize(mut x: Array1<f64>) -> Array1<f64> {
    let norm = l2_norm(x.view());
    x.mapv_inplace(|e| e/norm);
    x
}

fn main() {
    let x = array![1., 2., 3., 4., 5.];
    println!("||x||_2 = {}", l2_norm(x.view()));
    println!("||x||_1 = {}", l1_norm(x.view()));
    println!("Normalizing x yields {:?}", normalize(x));
}
```

[`Array1`]: https://docs.rs/ndarray/*/ndarray/type.Array1.html
[`ArrayView1`]: https://docs.rs/ndarray/*/ndarray/type.ArrayView1.html
[`dot`]: https://docs.rs/ndarray/*/ndarray/struct.ArrayBase.html#method.dot
[`fold`]: https://docs.rs/ndarray/*/ndarray/struct.ArrayBase.html#method.fold
[l1]: http://mathworld.wolfram.com/L1-Norm.html
[l2]: http://mathworld.wolfram.com/L2-Norm.html
