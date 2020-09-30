## 从基本 URL 创建新 URLs

<!--
> [web/url/new.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/url/new.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![url-badge]][url] [![cat-net-badge]][cat-net]

[`join`] 方法从基路径和相对路径创建新的 URL。

```rust,edition2018

use url::{Url, ParseError};

fn main() -> Result<(), ParseError> {
    let path = "/rust-lang/cargo";

    let gh = build_github_url(path)?;

    assert_eq!(gh.as_str(), "https://github.com/rust-lang/cargo");
    println!("The joined URL is: {}", gh);

    Ok(())
}

fn build_github_url(path: &str) -> Result<Url, ParseError> {
    const GITHUB: &'static str = "https://github.com";

    let base = Url::parse(GITHUB).expect("hardcoded URL is known to be valid");
    let joined = base.join(path)?;

    Ok(joined)
}
```

[`join`]: https://docs.rs/url/*/url/struct.Url.html#method.join
