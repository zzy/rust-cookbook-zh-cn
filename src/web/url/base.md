## 通过移除路径段创建基本 URL

<!--
> [web/url/base.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/url/base.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![url-badge]][url] [![cat-net-badge]][cat-net]

基本 URL（base URL）包括协议和域名。但基本 URL（base URL）不包括目录、文件或查询字符串，这些项都可以从给定的 URL 中剥离出来。创建基本 URL（base URL）时，通过 [`PathSegmentsMut::clear`] 方法移除目录和文件路径，通过方法 [`Url::set_query`] 移除查询字符串。

```rust,edition2018
# use error_chain::error_chain;

use url::Url;
#
# error_chain! {
#     foreign_links {
#         UrlParse(url::ParseError);
#     }
#     errors {
#         CannotBeABase
#     }
# }

fn main() -> Result<()> {
    let full = "https://github.com/rust-lang/cargo?asdf";

    let url = Url::parse(full)?;
    let base = base_url(url)?;

    assert_eq!(base.as_str(), "https://github.com/");
    println!("The base of the URL is: {}", base);

    Ok(())
}

fn base_url(mut url: Url) -> Result<Url> {
    match url.path_segments_mut() {
        Ok(mut path) => {
            path.clear();
        }
        Err(_) => {
            return Err(Error::from_kind(ErrorKind::CannotBeABase));
        }
    }

    url.set_query(None);

    Ok(url)
}
```

[`PathSegmentsMut::clear`]: https://docs.rs/url/*/url/struct.PathSegmentsMut.html#method.clear
[`Url::set_query`]: https://docs.rs/url/*/url/struct.Url.html#method.set_query
