## 用自定义环境变量设置日志记录

<!--
> [development_tools/debugging/config_log/log-env-variable.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/debugging/config_log/log-env-variable.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![log-badge]][log] [![env_logger-badge]][env_logger] [![cat-debugging-badge]][cat-debugging]

[`Builder`] 配置日志记录。

[`Builder::parse`] 以 [`RUST_LOG`] 语法的形式解析 `MY_APP_LOG` 环境变量的内容。然后，[`Builder::init`] 初始化记录器。所有这些步骤通常由 [`env_logger::init`] 在内部完成。

```rust,edition2018

use std::env;
use env_logger::Builder;

fn main() {
    Builder::new()
        .parse(&env::var("MY_APP_LOG").unwrap_or_default())
        .init();

    log::info!("informational message");
    log::warn!("warning message");
    log::error!("this is an error {}", "message");
}
```

[`env_logger::init`]: https://docs.rs/env_logger/*/env_logger/fn.init.html
[`Builder`]: https://docs.rs/env_logger/*/env_logger/struct.Builder.html
[`Builder::init`]: https://docs.rs/env_logger/*/env_logger/struct.Builder.html#method.init
[`Builder::parse`]: https://docs.rs/env_logger/*/env_logger/struct.Builder.html#method.parse
[`RUST_LOG`]: https://docs.rs/env_logger/*/env_logger/#enabling-logging
