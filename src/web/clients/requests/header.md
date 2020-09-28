## 为 REST 请求设置自定义消息标头和 URL 参数

<!--
> [web/clients/requests/header.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/clients/requests/header.md)
> <br />
> commit 203b1085212a7b857d9a29bdc6a763515e77e0f9 - 2020.06.08
-->

[![reqwest-badge]][reqwest] [![hyper-badge]][hyper] [![url-badge]][url] [![cat-net-badge]][cat-net]

本实例中为 HTTP GET 请求设置标准的和自定义的 HTTP 消息标头以及 URL 参数。使用 [`hyper::header!`] 宏创建 `XPoweredBy` 类型的自定义消息标头。

使用 [`Url::parse_with_params`] 构建复杂的 URL。使用 [`RequestBuilder::header`] 方法设置标准消息标头 [`header::UserAgent`]、[`header::Authorization`]，以及自定义类型 `XPoweredBy`，然后使用 [`RequestBuilder::send`] 发起请求。

请求的服务目标为 <http://httpbin.org/headers>，其响应结果是包含所有请求的消息标头的 JSON 字典，易于验证。

```rust,edition2018,no_run
# use error_chain::error_chain;
use serde::Deserialize;

use std::collections::HashMap;
use url::Url;
use reqwest::Client;
use reqwest::header::{UserAgent, Authorization, Bearer};

header! { (XPoweredBy, "X-Powered-By") => [String] }

#[derive(Deserialize, Debug)]
pub struct HeadersEcho {
    pub headers: HashMap<String, String>,
}
#
# error_chain! {
#     foreign_links {
#         Reqwest(reqwest::Error);
#         UrlParse(url::ParseError);
#     }
# }

fn main() -> Result<()> {
    let url = Url::parse_with_params("http://httpbin.org/headers",
                                     &[("lang", "rust"), ("browser", "servo")])?;

    let mut response = Client::new()
        .get(url)
        .header(UserAgent::new("Rust-test"))
        .header(Authorization(Bearer { token: "DEadBEEfc001cAFeEDEcafBAd".to_owned() }))
        .header(XPoweredBy("Guybrush Threepwood".to_owned()))
        .send()?;

    let out: HeadersEcho = response.json()?;
    assert_eq!(out.headers["Authorization"],
               "Bearer DEadBEEfc001cAFeEDEcafBAd");
    assert_eq!(out.headers["User-Agent"], "Rust-test");
    assert_eq!(out.headers["X-Powered-By"], "Guybrush Threepwood");
    assert_eq!(response.url().as_str(),
               "http://httpbin.org/headers?lang=rust&browser=servo");

    println!("{:?}", out);
    Ok(())
}
```

[`header::Authorization`]: https://doc.servo.org/hyper/header/struct.Authorization.html
[`header::UserAgent`]: https://doc.servo.org/hyper/header/struct.UserAgent.html
[`hyper::header!`]: https://doc.servo.org/hyper/macro.header.html
[`RequestBuilder::header`]: https://docs.rs/reqwest/*/reqwest/struct.RequestBuilder.html#method.header
[`RequestBuilder::send`]: https://docs.rs/reqwest/*/reqwest/struct.RequestBuilder.html#method.send
[`Url::parse_with_params`]: https://docs.rs/url/*/url/struct.Url.html#method.parse_with_params
