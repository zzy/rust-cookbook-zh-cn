## 发出 HTTP GET 请求

<!--
> [web/clients/requests/get.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/clients/requests/get.md)
> <br />
> commit dd4efa8dcd8e611326caa01c08db8f227aa909d6 - 2020.06.07
-->

[![reqwest-badge]][reqwest] [![cat-net-badge]][cat-net]

解析提供的 URL，并使用 [`reqwest::blocking::get`] 发起同步 HTTP GET 请求。打印获取的响应消息状态和标头 [`reqwest::blocking::Response`]。使用 [`read_to_string`] 将 HTTP 响应消息主体正文读入到指派的字符串 [`String`]。

```rust,edition2018,no_run
use error_chain::error_chain;
use std::io::Read;

error_chain! {
    foreign_links {
        Io(std::io::Error);
        HttpRequest(reqwest::Error);
    }
}

fn main() -> Result<()> {
    let mut res = reqwest::blocking::get("http://httpbin.org/get")?;
    let mut body = String::new();
    res.read_to_string(&mut body)?;

    println!("Status: {}", res.status());
    println!("Headers:\n{:#?}", res.headers());
    println!("Body:\n{}", body);

    Ok(())
}

```

### 异步

常见的方法是通过包含 [`tokio`] 在内的类似异步执行器，使主函数执行异步，但检索处理相同的信息。

本实例中，[`tokio::main`] 处理所有繁重的执行器设置，并允许在 `.await` 之前不阻塞的按顺序执行代码。

也可以使用 [reqwest](https://docs.rs/crate/reqwest/0.10.8) 的异步版本，其中 [`reqwest::get`] 和 [`reqwest::Response`] 都是异步的。

```rust,no_run
use error_chain::error_chain;

error_chain! {
    foreign_links {
        Io(std::io::Error);
        HttpRequest(reqwest::Error);
    }
}

#[tokio::main]
async fn main() -> Result<()> {
    let res = reqwest::get("http://httpbin.org/get").await?;
    println!("Status: {}", res.status());
    println!("Headers:\n{:#?}", res.headers());

    let body = res.text().await?;
    println!("Body:\n{}", body);
    Ok(())
}
```

[`read_to_string`]: https://doc.rust-lang.org/std/io/trait.Read.html#method.read_to_string
[`reqwest::blocking::get`]: https://docs.rs/reqwest/*/reqwest/blocking/fn.get.html
[`reqwest::blocking::Response`]: https://docs.rs/reqwest/*/reqwest/blocking/struct.Response.html
[`reqwest::get`]: https://docs.rs/reqwest/*/reqwest/fn.get.html
[`reqwest::Response`]: https://docs.rs/reqwest/*/reqwest/struct.Response.html
[`String`]: https://doc.rust-lang.org/std/string/struct.String.html
[`tokio`]: https://docs.rs/crate/tokio/0.2.11
[`tokio::main`]: https://tokio.rs/docs/getting-started/hello-world/#let-s-write-some-code