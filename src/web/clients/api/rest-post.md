## 使用 GitHub API 创建和删除 Gist

<!--
> [web/clients/api/rest-post.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/clients/api/rest-post.md)
> <br />
> commit dd4efa8dcd8e611326caa01c08db8f227aa909d6 - 2020.06.07
-->

[![reqwest-badge]][reqwest] [![serde-badge]][serde] [![cat-net-badge]][cat-net] [![cat-encoding-badge]][cat-encoding]

使用 [`Client::post`] 创建一个 POST 请求提交到 GitHub [gists API v3](https://docs.github.com/cn/free-pro-team@latest/rest/reference/gists) 接口的 gist，并使用 [`Client::delete`] 使用 DELETE 请求删除它。

[`reqwest::Client`] 负责这两个请求的详细信息，包括：URL、消息体（body）和身份验证。[`serde_json::json!`] 宏的 POST 主体可以提供任意形式的 JSON 主体，通过调用 [`RequestBuilder::json`] 设置请求主体，[`RequestBuilder::basic_auth`] 处理身份验证。本实例中调用 [`RequestBuilder::send`] 方法同步执行请求。

```rust,edition2018,no_run
use error_chain::error_chain;
use serde::Deserialize;
use serde_json::json;
use std::env;
use reqwest::Client;

error_chain! {
    foreign_links {
        EnvVar(env::VarError);
        HttpRequest(reqwest::Error);
    }
}

#[derive(Deserialize, Debug)]
struct Gist {
    id: String,
    html_url: String,
}

#[tokio::main]
async fn main() ->  Result<()> {
    let gh_user = env::var("GH_USER")?;
    let gh_pass = env::var("GH_PASS")?;

    let gist_body = json!({
        "description": "the description for this gist",
        "public": true,
        "files": {
             "main.rs": {
             "content": r#"fn main() { println!("hello world!");}"#
            }
        }});

    let request_url = "https://api.github.com/gists";
    let response = Client::new()
        .post(request_url)
        .basic_auth(gh_user.clone(), Some(gh_pass.clone()))
        .json(&gist_body)
        .send().await?;

    let gist: Gist = response.json().await?;
    println!("Created {:?}", gist);

    let request_url = format!("{}/{}",request_url, gist.id);
    let response = Client::new()
        .delete(&request_url)
        .basic_auth(gh_user, Some(gh_pass))
        .send().await?;

    println!("Gist {} deleted! Status code: {}",gist.id, response.status());
    Ok(())
}
```

实例中使用 [HTTP 基本认证][HTTP Basic Auth] 为了授权访问 [GitHub API]。实际应用中或许将使用一个更为复杂的 [OAuth] 授权流程。

[`Client::delete`]: https://docs.rs/reqwest/*/reqwest/struct.Client.html#method.delete
[`Client::post`]: https://docs.rs/reqwest/*/reqwest/struct.Client.html#method.post
[`RequestBuilder::basic_auth`]: https://docs.rs/reqwest/*/reqwest/struct.RequestBuilder.html#method.basic_auth
[`RequestBuilder::json`]: https://docs.rs/reqwest/*/reqwest/struct.RequestBuilder.html#method.json
[`RequestBuilder::send`]: https://docs.rs/reqwest/*/reqwest/struct.RequestBuilder.html#method.send
[`reqwest::Client`]: https://docs.rs/reqwest/*/reqwest/struct.Client.html
[`serde_json::json!`]: https://docs.rs/serde_json/*/serde_json/macro.json.html

[GitHub API]: https://docs.github.com/cn/free-pro-team@latest/rest/overview/other-authentication-methods
[HTTP Basic Auth]: https://tools.ietf.org/html/rfc2617
[OAuth]: https://oauth.net/getting-started/
