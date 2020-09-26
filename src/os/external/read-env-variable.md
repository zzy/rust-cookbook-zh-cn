## 读取环境变量

<!--
> [os/external/read-env-variable.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/os/external/read-env-variable.md)
> <br />
> commit bba147f18531934c904b1c5afaed3e6550b1c1c0 - 2020.06.14
-->

[![std-badge]][std] [![cat-os-badge]][cat-os]

通过 [std::env::var] 读取环境变量。

```rust,edition2018,no_run
use std::env;
use std::fs;
use std::io::Error;

fn main() -> Result<(), Error> {
    // 从环境变量 `CONFIG` 读取配置路径 `config_path`。
    // 如果 `CONFIG` 未设置，采用默认配置路径。
    let config_path = env::var("CONFIG")
        .unwrap_or("/etc/myapp/config".to_string());

    let config: String = fs::read_to_string(config_path)?;
    println!("Config: {}", config);

    Ok(())
}
```

[std::env::var]: https://doc.rust-lang.org/std/env/fn.var.html
