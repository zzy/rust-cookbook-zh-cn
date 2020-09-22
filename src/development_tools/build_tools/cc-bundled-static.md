## 编译并静态链接到绑定的 C 语言库

<!--
> [development_tools/build_tools/cc-bundled-static.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/build_tools/cc-bundled-static.md)
> <br />
> commit 203b1085212a7b857d9a29bdc6a763515e77e0f9 - 2020.06.08
-->

[![cc-badge]][cc] [![cat-development-tools-badge]][cat-development-tools]

为了适应项目中需要混合 C、C++，或 asm 等语言的场景，[**cc**][cc] crate 提供了一个简单的 API，用于将绑定的 C/C++/asm 代码编译成静态库（**.a**），静态库可以通过 **rustc** 静态链接。

下面的实例有一些绑定的 C 语言代码（**src/hello.c**），它们将从 rust 中调用。在编译 rust 源代码之前，**Cargo.toml** 中指定的“构建”文件（**build.rs**）预先运行。使用 [**cc**][cc] crate，将生成一个静态库文件（本例中为 **libhello.a**，请参阅 [`compile` 文档][cc-build-compile]），通过在 `extern` 代码块中声明外部函数签名，然后就可以从 rust 中调用该静态库。

本实例中绑定的 C 语言文件非常简单，只需要将一个源文件传递给 [`cc::Build`][cc-build]。对于更复杂的构建需求，[`cc::Build`][cc-build] 提供了一整套构建器方法，用于指定[`（包含）include`][cc-build-include]路径和扩展编译器[`标志（flag）`][cc-build-flag]。

### `Cargo.toml`

```toml
[package]
...
build = "build.rs"

[build-dependencies]
cc = "1"

[dependencies]
error-chain = "0.11"
```

### `build.rs`

```rust,edition2018,no_run
fn main() {
    cc::Build::new()
        .file("src/hello.c")
        .compile("hello");   // 输出 `libhello.a`
}
```

### `src/hello.c`

```c
#include <stdio.h>


void hello() {
    printf("Hello from C!\n");
}

void greet(const char* name) {
    printf("Hello, %s!\n", name);
}
```

### `src/main.rs`

```rust,edition2018,ignore
use error_chain::error_chain;
use std::ffi::CString;
use std::os::raw::c_char;

error_chain! {
    foreign_links {
        NulError(::std::ffi::NulError);
        Io(::std::io::Error);
    }
}
fn prompt(s: &str) -> Result<String> {
    use std::io::Write;
    print!("{}", s);
    std::io::stdout().flush()?;
    let mut input = String::new();
    std::io::stdin().read_line(&mut input)?;
    Ok(input.trim().to_string())
}

extern {
    fn hello();
    fn greet(name: *const c_char);
}

fn main() -> Result<()> {
    unsafe { hello() }
    let name = prompt("What's your name? ")?;
    let c_name = CString::new(name)?;
    unsafe { greet(c_name.as_ptr()) }
    Ok(())
}
```

[`cc::Build::define`]: https://docs.rs/cc/*/cc/struct.Build.html#method.define
[`Option`]: https://doc.rust-lang.org/std/option/enum.Option.html
[cc-build-compile]: https://docs.rs/cc/*/cc/struct.Build.html#method.compile
[cc-build-cpp]: https://docs.rs/cc/*/cc/struct.Build.html#method.cpp
[cc-build-flag]: https://docs.rs/cc/*/cc/struct.Build.html#method.flag
[cc-build-include]: https://docs.rs/cc/*/cc/struct.Build.html#method.include
[cc-build]: https://docs.rs/cc/*/cc/struct.Build.html
