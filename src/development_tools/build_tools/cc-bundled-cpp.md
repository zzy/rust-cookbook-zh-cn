## 编译并静态链接到绑定的 C++ 语言库

<!--
> [development_tools/build_tools/cc-bundled-cpp.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/build_tools/cc-bundled-cpp.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![cc-badge]][cc] [![cat-development-tools-badge]][cat-development-tools]

链接绑定的 C++ 语言库非常类似于链接绑定的 C 语言库。编译并静态链接绑定的 C++ 库时，与链接绑定的 C 语言库相比有两个核心区别：一是通过构造器方法 [`cpp(true)`][cc-build-cpp] 指定 C++ 编译器；二是通过在 C++ 源文件顶部添加 `extern "C"` 代码段，以防止 C++ 编译器的名称篡改。

### `Cargo.toml`

```toml
[package]
...
build = "build.rs"

[build-dependencies]
cc = "1"
```

### `build.rs`

```rust,edition2018,no_run
fn main() {
    cc::Build::new()
        .cpp(true)
        .file("src/foo.cpp")
        .compile("foo");   
}
```

### `src/foo.cpp`

```cpp
extern "C" {
    int multiply(int x, int y);
}

int multiply(int x, int y) {
    return x*y;
}
```

### `src/main.rs`

```rust,edition2018,ignore
extern {
    fn multiply(x : i32, y : i32) -> i32;
}

fn main(){
    unsafe {
        println!("{}", multiply(5,7));
    }   
}
```

[cc-build-cpp]: https://docs.rs/cc/*/cc/struct.Build.html#method.cpp
