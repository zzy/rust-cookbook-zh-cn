## 日期和 UNIX 时间戳的互相转换

<!--
> [datetime/parse/timestamp.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/datetime/parse/timestamp.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![chrono-badge]][chrono] [![cat-date-and-time-badge]][cat-date-and-time]

使用 [`NaiveDateTime::timestamp`] 将由 [`NaiveDate::from_ymd`] 生成的日期和由 [`NaiveTime::from_hms`] 生成的时间转换为 [UNIX 时间戳][UNIX timestamp]。然后，它使用 [`NaiveDateTime::from_timestamp`] 计算自 UTC 时间 1970 年 01 月 01 日 00:00:00 开始的 10 亿秒后的日期。

```rust,edition2018

use chrono::{NaiveDate, NaiveDateTime};

fn main() {
    let date_time: NaiveDateTime = NaiveDate::from_ymd(2017, 11, 12).and_hms(17, 33, 44);
    println!(
        "Number of seconds between 1970-01-01 00:00:00 and {} is {}.",
        date_time, date_time.timestamp());

    let date_time_after_a_billion_seconds = NaiveDateTime::from_timestamp(1_000_000_000, 0);
    println!(
        "Date after a billion seconds since 1970-01-01 00:00:00 was {}.",
        date_time_after_a_billion_seconds);
}
```

[`NaiveDate::from_ymd`]: https://docs.rs/chrono/*/chrono/naive/struct.NaiveDate.html#method.from_ymd
[`NaiveDateTime::from_timestamp`]: https://docs.rs/chrono/*/chrono/naive/struct.NaiveDateTime.html#method.from_timestamp
[`NaiveDateTime::timestamp`]: https://docs.rs/chrono/*/chrono/naive/struct.NaiveDateTime.html#method.timestamp
[`NaiveTime::from_hms`]: https://docs.rs/chrono/*/chrono/naive/struct.NaiveTime.html#method.from_hms

[UNIX timestamp]: https://en.wikipedia.org/wiki/Unix_time
