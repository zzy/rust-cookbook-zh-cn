## 记录到 Unix 系统日志

<!--
> [development_tools/debugging/log/log-syslog.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/debugging/log/log-syslog.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![log-badge]][log] [![syslog-badge]][syslog] [![cat-debugging-badge]][cat-debugging]

本实例实现将信息记录到 [UNIX syslog]。使用 [`syslog::init`] 初始化记录器后端。[`syslog::Facility`] 记录提交日志项分类的程序，[`log::LevelFilter`] 表示欲记录日志的等级，`Option<&str>` 定义应用程序名称（可选）。

```rust,edition2018
# #[cfg(target_os = "linux")]
# #[cfg(target_os = "linux")]
use syslog::{Facility, Error};

# #[cfg(target_os = "linux")]
fn main() -> Result<(), Error> {
    syslog::init(Facility::LOG_USER,
                 log::LevelFilter::Debug,
                 Some("My app name"))?;
    log::debug!("this is a debug {}", "message");
    log::error!("this is an error!");
    Ok(())
}

# #[cfg(not(target_os = "linux"))]
# fn main() {
#     println!("So far, only Linux systems are supported.");
# }
```

[`log::LevelFilter`]: https://docs.rs/log/*/log/enum.LevelFilter.html
[`syslog::Facility`]: https://docs.rs/syslog/*/syslog/enum.Facility.html
[`syslog::init`]: https://docs.rs/syslog/*/syslog/fn.init.html

[UNIX syslog]: https://www.gnu.org/software/libc/manual/html_node/Overview-of-Syslog.html
