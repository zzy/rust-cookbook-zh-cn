## 编译 C 语言库时自定义设置

<!--
> [development_tools/build_tools/cc-defines.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/build_tools/cc-defines.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![cc-badge]][cc] [![cat-development-tools-badge]][cat-development-tools]

使用 [`cc::Build::define`] 自定义构建绑定的 C 语言代码非常简单。该方法接受 [`Option`] 值，因此可以创建这样的定义：`#define APP_NAME "foo"`、`#define WELCOME`（将 `None` 作为不确定值传递）。如下实例构建了一个绑定的 C 语言文件，其在 `build.rs` 中设置了动态定义，并在运行时打印 “**Welcome to foo - version 1.0.2**”。Cargo 设定了一些[环境变量][cargo-env]，这些变量可能对某些自定义设置有用。

### `Cargo.toml`

```toml
[package]
...
version = "1.0.2"
build = "build.rs"

[build-dependencies]
cc = "1"
```

### `build.rs`

```rust,edition2018,no_run
fn main() {
    cc::Build::new()
        .define("APP_NAME", "\"foo\"")
        .define("VERSION", format!("\"{}\"", env!("CARGO_PKG_VERSION")).as_str())
        .define("WELCOME", None)
        .file("src/foo.c")
        .compile("foo");
}
```

### `src/foo.c`

```c
#include <stdio.h>

void print_app_info() {
#ifdef WELCOME
    printf("Welcome to ");
#endif
    printf("%s - version %s\n", APP_NAME, VERSION);
}
```

### `src/main.rs`

```rust,edition2018,ignore
extern {
    fn print_app_info();
}

fn main(){
    unsafe {
        print_app_info();
    }   
}
```

[cargo-env]: https://doc.rust-lang.org/cargo/reference/environment-variables.html
