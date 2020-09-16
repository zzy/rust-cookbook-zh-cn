## 记录调试信息到控制台

<!--
> [development_tools/debugging/log/log-debug.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/debugging/log/log-debug.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![log-badge]][log] [![env_logger-badge]][env_logger] [![cat-debugging-badge]][cat-debugging]

`log` crate 提供了日志工具，`env_logger` crate 通过环境变量配置日志记录。[`log::debug!`] 宏的工作方式类似于其它 [`std::fmt`] 格式化的字符串。

```rust,edition2018

fn execute_query(query: &str) {
    log::debug!("Executing query: {}", query);
}

fn main() {
    env_logger::init();

    execute_query("DROP TABLE students");
}
```

运行上述代码时，并没有输出信息被打印。因为默认情况下，日志级别为 `error`，任何较低级别的日志信息都将被忽略。

设置 `RUST_LOG` 环境变量以打印消息：

```
$ RUST_LOG=debug cargo run
```

Cargo 运行后，会在输出的最后一行打印出调试信息：

```
DEBUG:main: Executing query: DROP TABLE students
```

[`log::debug!`]: https://docs.rs/log/*/log/macro.debug.html
[`std::fmt`]: https://doc.rust-lang.org/std/fmt/
