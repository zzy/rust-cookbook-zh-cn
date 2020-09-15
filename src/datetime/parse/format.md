## 日期和时间的格式化显示

<!--
> [datetime/parse/format.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/datetime/parse/format.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![chrono-badge]][chrono] [![cat-date-and-time-badge]][cat-date-and-time]

使用 [`Utc::now`] 获取并显示当前 UTC 时间。使用 [`DateTime::to_rfc2822`] 将当前时间格式化为熟悉的 [RFC 2822] 格式，使用 [`DateTime::to_rfc3339`] 将当前时间格式化为熟悉的 [RFC 3339] 格式，也可以使用 [`DateTime::format`] 自定义时间格式。

```rust,edition2018
use chrono::{DateTime, Utc};

fn main() {
    let now: DateTime<Utc> = Utc::now();

    println!("UTC now is: {}", now);
    println!("UTC now in RFC 2822 is: {}", now.to_rfc2822());
    println!("UTC now in RFC 3339 is: {}", now.to_rfc3339());
    println!("UTC now in a custom format is: {}", now.format("%a %b %e %T %Y"));
}
```

[`DateTime::format`]: https://docs.rs/chrono/*/chrono/struct.DateTime.html#method.format
[`DateTime::to_rfc2822`]: https://docs.rs/chrono/*/chrono/struct.DateTime.html#method.to_rfc2822
[`DateTime::to_rfc3339`]: https://docs.rs/chrono/*/chrono/struct.DateTime.html#method.to_rfc3339
[`Utc::now`]: https://docs.rs/chrono/*/chrono/offset/struct.Utc.html#method.now

[RFC 2822]: https://www.ietf.org/rfc/rfc2822.txt
[RFC 3339]: https://www.ietf.org/rfc/rfc3339.txt
