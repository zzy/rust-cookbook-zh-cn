## 测量运行时间

<!--
> [datetime/duration/profile.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/datetime/duration/profile.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std] [![cat-time-badge]][cat-time]

测量从 [`time::Instant::now`] 开始运行的时间 [`time::Instant::elapsed`]。

调用 [`time::Instant::elapsed`] 将返回 [`time::Duration`]，我们将在实例末尾打印该时间。此方法不会更改或者重置 [`time::Instant`] 对象。

```rust,edition2018
use std::time::{Duration, Instant};
# use std::thread;
#
# fn expensive_function() {
#     thread::sleep(Duration::from_secs(1));
# }

fn main() {
    let start = Instant::now();
    expensive_function();
    let duration = start.elapsed();

    println!("Time elapsed in expensive_function() is: {:?}", duration);
}
```

[`time::Duration`]: https://doc.rust-lang.org/std/time/struct.Duration.html
[`time::Instant::elapsed`]: https://doc.rust-lang.org/std/time/struct.Instant.html#method.elapsed
[`time::Instant::now`]: https://doc.rust-lang.org/std/time/struct.Instant.html#method.now
[`time::Instant`]:https://doc.rust-lang.org/std/time/struct.Instant.html
