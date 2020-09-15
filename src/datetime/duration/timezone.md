## 时间的时区转换

<!--
> [datetime/duration/timezone.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/datetime/duration/timezone.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![chrono-badge]][chrono] [![cat-date-and-time-badge]][cat-date-and-time]

使用 [`offset::Local::now`] 获取本地时间并显示，然后使用 [`DateTime::from_utc`] 结构体方法将其转换为 UTC 标准格式。最后，使用 [`offset::FixedOffset`] 结构体，可以将 UTC 时间转换为 UTC+8 和 UTC-2。

```rust,edition2018

use chrono::{DateTime, FixedOffset, Local, Utc};

fn main() {
    let local_time = Local::now();
    let utc_time = DateTime::<Utc>::from_utc(local_time.naive_utc(), Utc);
    let china_timezone = FixedOffset::east(8 * 3600);
    let rio_timezone = FixedOffset::west(2 * 3600);
    println!("Local time now is {}", local_time);
    println!("UTC time now is {}", utc_time);
    println!(
        "Time in Hong Kong now is {}",
        utc_time.with_timezone(&china_timezone)
    );
    println!("Time in Rio de Janeiro now is {}", utc_time.with_timezone(&rio_timezone));
}
```

[`DateTime::from_utc`]:https://docs.rs/chrono/*/chrono/struct.DateTime.html#method.from_utc
[`offset::FixedOffset`]: https://docs.rs/chrono/*/chrono/offset/struct.FixedOffset.html
[`offset::Local::now`]: https://docs.rs/chrono/*/chrono/offset/struct.Local.html#method.now
