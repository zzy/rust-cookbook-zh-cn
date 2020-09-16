## 记录信息时，用标准输出 stdout 替换标准错误 stderr

<!--
> [development_tools/debugging/log/log-stdout.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/debugging/log/log-stdout.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![log-badge]][log] [![env_logger-badge]][env_logger] [![cat-debugging-badge]][cat-debugging]

使用 [`Builder::target`] 创建自定义的日志记录器配置，将日志输出的目标设置为 [`Target::Stdout`]。

```rust,edition2018

use env_logger::{Builder, Target};

fn main() {
    Builder::new()
        .target(Target::Stdout)
        .init();

    log::error!("This error has been printed to Stdout");
}
```

[`Builder::target`]: https://docs.rs/env_logger/*/env_logger/struct.Builder.html#method.target
[`Target::Stdout`]: https://docs.rs/env_logger/*/env_logger/fmt/enum.Target.html
