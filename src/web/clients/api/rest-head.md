## 检查 API 资源是否存在

<!--
> [web/clients/api/rest-head.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/clients/api/rest-head.md)
> <br />
> commit 67329add00792d6cfc04b5d4cfb073a5fb12ba53 - 2020.06.14
-->

[![reqwest-badge]][reqwest] [![cat-net-badge]][cat-net]

使用消息标头 HEAD 请求（([`Client::head`]）查询 GitHub 用户端接口，然后检查响应代码以确定是否成功。这是一种无需接收 HTTP 响应消息主体，即可快速查询 rest 资源的方法。使用 [`ClientBuilder::timeout`] 方法配置的 [`reqwest::Client`] 结构体将确保请求不会超时。

由于 [`ClientBuilder::build`] 和 [`RequestBuilder::send`] 都返回错误类型 [`reqwest::Error`]，所以便捷的 [`reqwest::Result`] 类型被用于主函数的返回类型。

```rust,edition2018,no_run
use reqwest::Result;
use std::time::Duration;
use reqwest::ClientBuilder;

#[tokio::main]
async fn main() -> Result<()> {
    let user = "ferris-the-crab";
    let request_url = format!("https://api.github.com/users/{}", user);
    println!("{}", request_url);

    let timeout = Duration::new(5, 0);
    let client = ClientBuilder::new().timeout(timeout).build()?;
    let response = client.head(&request_url).send().await?;

    if response.status().is_success() {
        println!("{} is a user!", user);
    } else {
        println!("{} is not a user!", user);
    }

    Ok(())
}
```

[`ClientBuilder::build`]: https://docs.rs/reqwest/*/reqwest/struct.ClientBuilder.html#method.build
[`Client::head`]: https://docs.rs/reqwest/*/reqwest/struct.Client.html#method.head
[`ClientBuilder::timeout`]: https://docs.rs/reqwest/*/reqwest/struct.ClientBuilder.html#method.timeout
[`RequestBuilder::send`]: https://docs.rs/reqwest/*/reqwest/struct.RequestBuilder.html#method.send
[`reqwest::Client`]: https://docs.rs/reqwest/*/reqwest/struct.Client.html
[`reqwest::Error`]: https://docs.rs/reqwest/*/reqwest/struct.Error.html
[`reqwest::Result`]:https://docs.rs/reqwest/*/reqwest/type.Result.html
