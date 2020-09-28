## 查询 GitHub API

<!--
> [web/clients/api/rest-get.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/clients/api/rest-get.md)
> <br />
> commit dd4efa8dcd8e611326caa01c08db8f227aa909d6 - 2020.06.07
-->

[![reqwest-badge]][reqwest] [![serde-badge]][serde] [![cat-net-badge]][cat-net] [![cat-encoding-badge]][cat-encoding]

使用 [`reqwest::get`] 查询 [点赞的用户 API v3](https://docs.github.com/cn/free-pro-team@latest/rest/reference/activity#list-stargazers)，以获取某个 GitHub 项目的所有点赞用户的列表。使用 [`Response::json`] 将响应信息 [`reqwest::Response`] 反序列化为实现了 [`serde::Deserialize`] trait 的 `User` 对象。

[tokio::main] 用于设置异步执行器，该进程异步等待 [`reqwest::get`] 完成，然后将响应信息反序列化到用户实例中。

```rust,edition2018,no_run
use serde::Deserialize;
use reqwest::Error;

#[derive(Deserialize, Debug)]
struct User {
    login: String,
    id: u32,
}

#[tokio::main]
async fn main() -> Result<(), Error> {
    let request_url = format!("https://api.github.com/repos/{owner}/{repo}/stargazers",
                              owner = "rust-lang-nursery",
                              repo = "rust-cookbook");
    println!("{}", request_url);
    let response = reqwest::get(&request_url).await?;

    let users: Vec<User> = response.json().await?;
    println!("{:?}", users);
    Ok(())
}
```

[`reqwest::get`]: https://docs.rs/reqwest/*/reqwest/fn.get.html
[`reqwest::Response`]: https://docs.rs/reqwest/*/reqwest/struct.Response.html
[`Response::json`]: https://docs.rs/reqwest/*/reqwest/struct.Response.html#method.json
[`serde::Deserialize`]: https://docs.rs/serde/*/serde/trait.Deserialize.html
[tokio::main]: https://docs.rs/tokio/0.2.22/tokio/attr.main.html