## 启用每个模块的日志级别

<!--
> [development_tools/debugging/config_log/log-mod.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/debugging/config_log/log-mod.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![log-badge]][log] [![env_logger-badge]][env_logger] [![cat-debugging-badge]][cat-debugging]

创建两个模块：`foo` 和其嵌套的 `foo::bar`，日志记录指令分别由 [`RUST_LOG`] 环境变量控制。

```rust,edition2018

mod foo {
    mod bar {
        pub fn run() {
            log::warn!("[bar] warn");
            log::info!("[bar] info");
            log::debug!("[bar] debug");
        }
    }

    pub fn run() {
        log::warn!("[foo] warn");
        log::info!("[foo] info");
        log::debug!("[foo] debug");
        bar::run();
    }
}

fn main() {
    env_logger::init();
    log::warn!("[root] warn");
    log::info!("[root] info");
    log::debug!("[root] debug");
    foo::run();
}
```

[`RUST_LOG`] 环境变量控制 [`env_logger`][env_logger] 的输出。模块声明采用逗号分隔各项，格式类似于 `path::to::module=log_level`。按如下方式运行 `test` 应用程序：

```bash
RUST_LOG="warn,test::foo=info,test::foo::bar=debug" ./test
```

将日志等级 [`log::Level`] 的默认值设置为 `warn`，将模块 `foo` 和其嵌套的模块 `foo::bar` 的日志等级设置为 `info` 和 `debug`。

```bash
WARN:test: [root] warn
WARN:test::foo: [foo] warn
INFO:test::foo: [foo] info
WARN:test::foo::bar: [bar] warn
INFO:test::foo::bar: [bar] info
DEBUG:test::foo::bar: [bar] debug
```

[`log::Level`]: https://docs.rs/log/*/log/enum.Level.html
[`RUST_LOG`]: https://docs.rs/env_logger/*/env_logger/#enabling-logging
