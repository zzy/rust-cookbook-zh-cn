## 生成短期线程

[![crossbeam-badge]][crossbeam] [![cat-concurrency-badge]][cat-concurrency]

本实例使用 [crossbeam] crate 为并发和并行编程提供了数据结构和函数。[`Scope::spawn`] 生成一个新的作用域线程，该线程确保传入 [`crossbeam::scope`] 函数的闭包在返回之前终止，这意味着您可以从调用的函数中引用数据。

本实例将数组一分为二，并在不同的线程中并行计算。

```rust,edition2018
fn main() {
    let arr = &[1, 25, -4, 10];
    let max = find_max(arr);
    assert_eq!(max, Some(25));
}

fn find_max(arr: &[i32]) -> Option<i32> {
    const THRESHOLD: usize = 2;
  
    if arr.len() <= THRESHOLD {
        return arr.iter().cloned().max();
    }

    let mid = arr.len() / 2;
    let (left, right) = arr.split_at(mid);
  
    crossbeam::scope(|s| {
        let thread_l = s.spawn(|_| find_max(left));
        let thread_r = s.spawn(|_| find_max(right));
  
        let max_l = thread_l.join().unwrap()?;
        let max_r = thread_r.join().unwrap()?;
  
        Some(max_l.max(max_r))
    }).unwrap()
}
```

[`crossbeam::scope`]: https://docs.rs/crossbeam/*/crossbeam/fn.scope.html
[`Scope::spawn`]: https://docs.rs/crossbeam/*/crossbeam/thread/struct.Scope.html#method.spawn
