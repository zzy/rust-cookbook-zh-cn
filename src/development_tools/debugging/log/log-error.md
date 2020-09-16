## 记录错误信息到控制台

<!--
> [development_tools/debugging/log/log-error.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/debugging/log/log-error.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![log-badge]][log] [![env_logger-badge]][env_logger] [![cat-debugging-badge]][cat-debugging]

正确的错误处理会将异常视为错误。下述实例中，通过 `log` 便捷宏 [`log::error!`]，将错误记录到 stderr。

```rust,edition2018

fn execute_query(_query: &str) -> Result<(), &'static str> {
    Err("I'm afraid I can't do that")
}

fn main() {
    env_logger::init();

    let response = execute_query("DROP TABLE students");
    if let Err(err) = response {
        log::error!("Failed to execute query: {}", err);
    }
}
```

[`log::error!`]: https://docs.rs/log/*/log/macro.error.html
