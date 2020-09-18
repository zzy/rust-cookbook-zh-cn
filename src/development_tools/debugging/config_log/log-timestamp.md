## 在日志信息中包含时间戳

<!--
> [development_tools/debugging/config_log/log-timestamp.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/debugging/config_log/log-timestamp.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![log-badge]][log] [![env_logger-badge]][env_logger] [![chrono-badge]][chrono] [![cat-debugging-badge]][cat-debugging]

使用 [`Builder`] 创建自定义记录器配置。每个日志项调用 [`Local::now`] 以获取本地时区中的当前 [`DateTime`]，并使用 [`DateTime::format`] 和 [`strftime::specifiers`] 来格式化最终日志中使用的时间戳。

如下实例调用 [`Builder::format`] 设置一个闭包，该闭包用时间戳、[`Record::level`] 和正文（[`Record::args`]）对每个信息文本进行格式化。

```rust,edition2018
use std::io::Write;
use chrono::Local;
use env_logger::Builder;
use log::LevelFilter;

fn main() {
    Builder::new()
        .format(|buf, record| {
            writeln!(buf,
                "{} [{}] - {}",
                Local::now().format("%Y-%m-%dT%H:%M:%S"),
                record.level(),
                record.args()
            )
        })
        .filter(None, LevelFilter::Info)
        .init();

    log::warn!("warn");
    log::info!("info");
    log::debug!("debug");
}
```
stderr output will contain
```
2017-05-22T21:57:06 [WARN] - warn
2017-05-22T21:57:06 [INFO] - info
```

[`DateTime::format`]: https://docs.rs/chrono/*/chrono/struct.DateTime.html#method.format
[`DateTime`]: https://docs.rs/chrono/*/chrono/datetime/struct.DateTime.html
[`Local::now`]: https://docs.rs/chrono/*/chrono/offset/struct.Local.html#method.now
[`Builder`]: https://docs.rs/env_logger/*/env_logger/struct.Builder.html
[`Builder::format`]: https://docs.rs/env_logger/*/env_logger/struct.Builder.html#method.format
[`Record::args`]: https://docs.rs/log/*/log/struct.Record.html#method.args
[`Record::level`]: https://docs.rs/log/*/log/struct.Record.html#method.level
[`strftime::specifiers`]: https://docs.rs/chrono/*/chrono/format/strftime/index.html#specifiers
