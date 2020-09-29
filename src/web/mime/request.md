## 解析 HTTP 响应的 MIME 类型

<!--
> [web/mime/request.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/mime/request.md)
> <br />
> commit dd4efa8dcd8e611326caa01c08db8f227aa909d6 - 2020.06.07
-->

[![reqwest-badge]][reqwest] [![mime-badge]][mime] [![cat-net-badge]][cat-net] [![cat-encoding-badge]][cat-encoding]

当从 `reqwest` 接收到 HTTP 响应时，[MIME 类型][MIME type]或媒体类型可以在实体头部的 [Content-Type] 标头中找到。[`reqwest::header::HeaderMap::get`] 方法将标头检索为结构体 [`reqwest::header::HeaderValue`]，结构体可以转换为字符串。然后 `mime` crate 可以解析它，生成 [`mime::Mime`] 值。

[`mime`] crate 也定义了一些常用的 MIME 类型。

请注意：[`reqwest::header`] 模块是从 [`http`] crate 导出的。

```rust,edition2018,no_run
use error_chain::error_chain;
use mime::Mime;
use std::str::FromStr;
use reqwest::header::CONTENT_TYPE;

 error_chain! {
    foreign_links {
        Reqwest(reqwest::Error);
        Header(reqwest::header::ToStrError);
        Mime(mime::FromStrError);
    }
 }

#[tokio::main]
async fn main() -> Result<()> {
    let response = reqwest::get("https://www.rust-lang.org/logos/rust-logo-32x32.png").await?;
    let headers = response.headers();

    match headers.get(CONTENT_TYPE) {
        None => {
            println!("The response does not contain a Content-Type header.");
        }
        Some(content_type) => {
            let content_type = Mime::from_str(content_type.to_str()?)?;
            let media_type = match (content_type.type_(), content_type.subtype()) {
                (mime::TEXT, mime::HTML) => "a HTML document",
                (mime::TEXT, _) => "a text document",
                (mime::IMAGE, mime::PNG) => "a PNG image",
                (mime::IMAGE, _) => "an image",
                _ => "neither text nor image",
            };

            println!("The reponse contains {}.", media_type);
        }
    };

    Ok(())
}
```

[`http`]: https://docs.rs/http/*/http/
[`mime::Mime`]: https://docs.rs/mime/*/mime/struct.Mime.html
[`reqwest::header::HeaderMap::get`]: https://docs.rs/reqwest/*/reqwest/header/struct.HeaderMap.html#method.get
[`reqwest::header::HeaderValue`]: https://docs.rs/reqwest/*/reqwest/header/struct.HeaderValue.html
[`reqwest::header`]: https://docs.rs/reqwest/*/reqwest/header/index.html

[Content-Type]: https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type
[MIME type]: https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types