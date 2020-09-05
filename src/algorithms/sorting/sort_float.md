## 浮点数 Vector 排序

<!--
> [algorithms/sorting/sort_float.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/algorithms/sorting/sort_float.md)
> <br />
> commit - b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std] [![cat-science-badge]][cat-science]

f32 或 f64 的 vector，可以使用 [`vec::sort_by`] 和 [`PartialOrd::partial_cmp`] 对其进行排序。

```rust,edition2018
fn main() {
    let mut vec = vec![1.1, 1.15, 5.5, 1.123, 2.0];

    vec.sort_by(|a, b| a.partial_cmp(b).unwrap());

    assert_eq!(vec, vec![1.1, 1.123, 1.15, 2.0, 5.5]);
}
```

[`vec::sort_by`]: https://doc.rust-lang.org/std/primitive.slice.html#method.sort_by
[`PartialOrd::partial_cmp`]: https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html#tymethod.partial_cmp
