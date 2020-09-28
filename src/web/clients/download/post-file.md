## POST 文件到 paste-rs

<!--
> [web/clients/download/post-file.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/clients/download/post-file.md)
> <br />
> commit dd4efa8dcd8e611326caa01c08db8f227aa909d6 - 2020.06.07
-->

[![reqwest-badge]][reqwest] [![cat-net-badge]][cat-net]

本实例使用 [`reqwest::Client`] 建立与 https://paste.rs 的连接，遵循 [`reqwest::RequestBuilder`] 结构体模式。调用 [`Client::post`] 方法，以 URL 为参数连接目标，[`RequestBuilder::body`] 通过读取文件设置要发送的内容，[`RequestBuilder::send`] 方法在文件上传过程中将一直阻塞，直到返回响应消息。最后，[`read_to_string`] 返回响应消息并显示在控制台中。

```rust,edition2018,no_run
use error_chain::error_chain;
use std::fs::File;
use std::io::Read;

 error_chain! {
     foreign_links {
         HttpRequest(reqwest::Error);
         IoError(::std::io::Error);
     }
 }
 #[tokio::main]

async fn main() -> Result<()> {
    let paste_api = "https://paste.rs";
    let mut file = File::open("message")?;

    let mut contents = String::new();
    file.read_to_string(&mut contents)?;

    let client = reqwest::Client::new();
    let res = client.post(paste_api)
        .body(contents)
        .send()
        .await?;
    let response_text = res.text().await?;
    println!("Your paste is located at: {}",response_text );
    Ok(())
}
```

[`Client::post`]: https://docs.rs/reqwest/*/reqwest/struct.Client.html#method.post
[`read_to_string`]: https://doc.rust-lang.org/std/io/trait.Read.html#method.read_to_string
[`RequestBuilder::body`]: https://docs.rs/reqwest/*/reqwest/struct.RequestBuilder.html#method.body
[`RequestBuilder::send`]: https://docs.rs/reqwest/*/reqwest/struct.RequestBuilder.html#method.send
[`reqwest::Client`]: https://docs.rs/reqwest/*/reqwest/struct.Client.html
[`reqwest::RequestBuilder`]: https://docs.rs/reqwest/*/reqwest/struct.RequestBuilder.html
