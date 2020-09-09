## 保持全局可变状态

<!--
> [concurrency/thread/global-mut-state.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/concurrency/thread/global-mut-state.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![lazy_static-badge]][lazy_static] [![cat-rust-patterns-badge]][cat-rust-patterns]

使用 [lazy_static] 声明全局状态。[lazy_static] 创建了一个全局可用的 `static ref`，它需要 [`Mutex`] 来允许变化（请参阅 [`RwLock`]）。在 [`Mutex`] 的包裹下，保证了状态不能被多个线程同时访问，从而防止出现争用情况。必须获取 [`MutexGuard`]，方可读取或更改存储在 [`Mutex`] 中的值。

```rust,edition2018
# use error_chain::error_chain;
use lazy_static::lazy_static;
use std::sync::Mutex;
#
# error_chain!{ }

lazy_static! {
    static ref FRUIT: Mutex<Vec<String>> = Mutex::new(Vec::new());
}

fn insert(fruit: &str) -> Result<()> {
    let mut db = FRUIT.lock().map_err(|_| "Failed to acquire MutexGuard")?;
    db.push(fruit.to_string());
    Ok(())
}

fn main() -> Result<()> {
    insert("apple")?;
    insert("orange")?;
    insert("peach")?;
    {
        let db = FRUIT.lock().map_err(|_| "Failed to acquire MutexGuard")?;

        db.iter().enumerate().for_each(|(i, item)| println!("{}: {}", i, item));
    }
    insert("grape")?;
    Ok(())
}
```

[`Mutex`]: https://doc.rust-lang.org/std/sync/struct.Mutex.html
[`MutexGuard`]: https://doc.rust-lang.org/std/sync/struct.MutexGuard.html
[`RwLock`]: https://doc.rust-lang.org/std/sync/struct.RwLock.html
