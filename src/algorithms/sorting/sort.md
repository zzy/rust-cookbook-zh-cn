## 整数 Vector 排序

<!--
> [algorithms/sorting/sort.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/algorithms/sorting/sort.md)
> <br />
> commit - b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std] [![cat-science-badge]][cat-science]

这个实例通过 [`vec::sort`] 对一个整数 Vector 进行排序。另一种方法是使用 [`vec::sort_unstable`]，后者运行速度更快一些，但不保持相等元素的顺序。

```rust,edition2018
fn main() {
    let mut vec = vec![1, 5, 10, 2, 15];
    
    vec.sort();

    assert_eq!(vec, vec![1, 2, 5, 10, 15]);
}
```

[`vec::sort`]: https://doc.rust-lang.org/std/vec/struct.Vec.html#method.sort
[`vec::sort_unstable`]: https://doc.rust-lang.org/std/vec/struct.Vec.html#method.sort_unstable
