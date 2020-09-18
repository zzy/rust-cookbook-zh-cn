## 将信息记录到自定义位置

<!--
> [development_tools/debugging/config_log/log-custom.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/debugging/config_log/log-custom.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![log-badge]][log] [![log4rs-badge]][log4rs] [![cat-debugging-badge]][cat-debugging]

[log4rs] 将日志输出配置到自定义位置。[log4rs] 可以使用外部 YAML 文件或生成器配置。

使用文件附加器 [`log4rs::append::file::FileAppender`] 创建日志配置，文件附加器定义日志记录的目标位置。日志配置使用 [`log4rs::encode::pattern`] 中的自定义模式进行编码，将配置项分配给 [`log4rs::config::Config`]，并设置默认的日志等级 [`log::LevelFilter`]。

```rust,edition2018,no_run
# use error_chain::error_chain;

use log::LevelFilter;
use log4rs::append::file::FileAppender;
use log4rs::encode::pattern::PatternEncoder;
use log4rs::config::{Appender, Config, Root};
#
# error_chain! {
#     foreign_links {
#         Io(std::io::Error);
#         LogConfig(log4rs::config::Errors);
#         SetLogger(log::SetLoggerError);
#     }
# }

fn main() -> Result<()> {
    let logfile = FileAppender::builder()
        .encoder(Box::new(PatternEncoder::new("{l} - {m}\n")))
        .build("log/output.log")?;

    let config = Config::builder()
        .appender(Appender::builder().build("logfile", Box::new(logfile)))
        .build(Root::builder()
                   .appender("logfile")
                   .build(LevelFilter::Info))?;

    log4rs::init_config(config)?;

    log::info!("Hello, world!");

    Ok(())
}
```

[`log4rs::append::file::FileAppender`]: https://docs.rs/log4rs/*/log4rs/append/file/struct.FileAppender.html
[`log4rs::config::Config`]: https://docs.rs/log4rs/*/log4rs/config/struct.Config.html
[`log4rs::encode::pattern`]: https://docs.rs/log4rs/*/log4rs/encode/pattern/index.html
[`log::LevelFilter`]: https://docs.rs/log/*/log/enum.LevelFilter.html
