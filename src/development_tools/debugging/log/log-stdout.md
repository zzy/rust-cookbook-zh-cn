## 记录信息时，用标准输出 stdout 替换标准错误 stderr

<!--
> [development_tools/debugging/log/log-error.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/debugging/log/log-error.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![log-badge]][log] [![env_logger-badge]][env_logger] [![cat-debugging-badge]][cat-debugging]

Creates a custom logger configuration using the [`Builder::target`] to set the target of the log output to [`Target::Stdout`].

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
