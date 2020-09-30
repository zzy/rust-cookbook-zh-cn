## 检查网页死链

<!--
> [web/scraping/broken.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/scraping/broken.md)
> <br />
> commit 203b1085212a7b857d9a29bdc6a763515e77e0f9 - 2020.06.08
-->

[![reqwest-badge]][reqwest] [![select-badge]][select] [![url-badge]][url] [![cat-net-badge]][cat-net]

调用 `get_base_url` 方法检索 `base URL`，如果 HTML 文档有 `base` 标签，从 `base` 标记获取 href [`attr`]，初始 URL 的默认值是 [`Position::BeforePath`]。

遍历 HTML 文档中的链接，并创建一个 [`tokio::spawn`] 任务，该任务将使用 [`url::ParseOptions`] 结构体和 [`Url::parse`] 方法解析单个链接。任务执行中，使用 [reqwest] 向链接发起请求，并验证状态码结构体 [`StatusCode`]。实例中使用 `await` 异步等待任务完成，然后结束程序。

```rust,edition2018,no_run
use error_chain::error_chain;
use reqwest::StatusCode;
use select::document::Document;
use select::predicate::Name;
use std::collections::HashSet;
use url::{Position, Url};

error_chain! {
  foreign_links {
      ReqError(reqwest::Error);
      IoError(std::io::Error);
      UrlParseError(url::ParseError);
      JoinError(tokio::task::JoinError);
  }
}

async fn get_base_url(url: &Url, doc: &Document) -> Result<Url> {
  let base_tag_href = doc.find(Name("base")).filter_map(|n| n.attr("href")).nth(0);
  let base_url =
    base_tag_href.map_or_else(|| Url::parse(&url[..Position::BeforePath]), Url::parse)?;
  Ok(base_url)
}

async fn check_link(url: &Url) -> Result<bool> {
  let res = reqwest::get(url.as_ref()).await?;
  Ok(res.status() != StatusCode::NOT_FOUND)
}

#[tokio::main]
async fn main() -> Result<()> {
  let url = Url::parse("https://www.rust-lang.org/en-US/")?;
  let res = reqwest::get(url.as_ref()).await?.text().await?;
  let document = Document::from(res.as_str());
  let base_url = get_base_url(&url, &document).await?;
  let base_parser = Url::options().base_url(Some(&base_url));
  let links: HashSet<Url> = document
    .find(Name("a"))
    .filter_map(|n| n.attr("href"))
    .filter_map(|link| base_parser.parse(link).ok())
    .collect();
    let mut tasks = vec![];

    for link in links {
        tasks.push(tokio::spawn(async move {
            if check_link(&link).await.unwrap() {
                println!("{} is OK", link);
            } else {
                println!("{} is Broken", link);
            }
        }));
    }

    for task in tasks {
        task.await?
    }

  Ok(())
}
```

[`attr`]: https://docs.rs/select/*/select/node/struct.Node.html#method.attr
[`Position::BeforePath`]: https://docs.rs/url/*/url/enum.Position.html#variant.BeforePath
[`StatusCode`]: https://docs.rs/reqwest/*/reqwest/struct.StatusCode.html
[`tokio::spawn`]: https://docs.rs/tokio/*/tokio/fn.spawn.html
[`url::Parse`]: https://docs.rs/url/*/url/struct.Url.html#method.parse
[`url::ParseOptions`]: https://docs.rs/url/*/url/struct.ParseOptions.html
