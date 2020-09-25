## 在 main 方法中对错误适当处理

<!--
> [errors/handle/main.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/errors/handle/main.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![error-chain-badge]][error-chain] [![cat-rust-patterns-badge]][cat-rust-patterns]

处理尝试打开不存在的文件时发生的错误，是通过使用 [error-chain] crate 来实现的。[error-chain] crate 包含大量的模板代码，用于 [Rust 中的错误处理]。

[`foreign_links`] 代码块内的 `Io(std::io::Error)` 函数允许由 [`std::io::Error`] 所报错误信息到 [`error_chain!`] 所定义错误类型的自动转换，[`error_chain!`] 所定义错误类型将实现 [`Error`] trait。

下文的实例将通过打开 Unix 文件 `/proc/uptime` 并解析内容以获得其中第一个数字，从而告诉系统运行了多长时间。除非出现错误，否则返回正常运行时间。

本书中的其他实例将隐藏 [error-chain] 模板，如果需要查看，可以通过 ⤢ 按钮展开代码。

```rust,edition2018
use error_chain::error_chain;

use std::fs::File;
use std::io::Read;

error_chain!{
    foreign_links {
        Io(std::io::Error);
        ParseInt(::std::num::ParseIntError);
    }
}

fn read_uptime() -> Result<u64> {
    let mut uptime = String::new();
    File::open("/proc/uptime")?.read_to_string(&mut uptime)?;

    Ok(uptime
        .split('.')
        .next()
        .ok_or("Cannot parse uptime data")?
        .parse()?)
}

fn main() {
    match read_uptime() {
        Ok(uptime) => println!("uptime: {} seconds", uptime),
        Err(err) => eprintln!("error: {}", err),
    };
}
```

[`error_chain!`]: https://docs.rs/error-chain/*/error_chain/macro.error_chain.html
[`Error`]: https://doc.rust-lang.org/std/error/trait.Error.html
[`foreign_links`]: https://docs.rs/error-chain/*/error_chain/#foreign-links
[`std::io::Error`]: https://doc.rust-lang.org/std/io/struct.Error.html

[Rust 中的错误处理]: https://rust-lang.budshome.com/ch09-00-error-handling.html
