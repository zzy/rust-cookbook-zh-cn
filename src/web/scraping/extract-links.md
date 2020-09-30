## 从 HTML 网页中提取所有链接

<!--
> [web/scraping/extract-links.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/scraping/extract-links.md)
> <br />
> commit dd4efa8dcd8e611326caa01c08db8f227aa909d6 - 2020.06.07
-->

[![reqwest-badge]][reqwest] [![select-badge]][select] [![cat-net-badge]][cat-net]

使用 [`reqwest::get`] 执行 HTTP GET 请求，然后使用 [`Document::from_read`] 将响应信息解析为 HTML 文档。以“a”（锚元素）作为结构体 [`Name`] 的参数，将结构体 [`Name`] 作为条件，使用 [`find`] 方法检索所有链接。在结构体 [`Selection`] 上调用 [`filter_map`] 方法，从具有 “href” [`attr`]（属性）的链接检索所有 URL。

```rust,edition2018,no_run
use error_chain::error_chain;
use select::document::Document;
use select::predicate::Name;

error_chain! {
      foreign_links {
          ReqError(reqwest::Error);
          IoError(std::io::Error);
      }
}

#[tokio::main]
async fn main() -> Result<()> {
  let res = reqwest::get("https://www.rust-lang.org/en-US/")
    .await?
    .text()
    .await?;

  Document::from(res.as_str())
    .find(Name("a"))
    .filter_map(|n| n.attr("href"))
    .for_each(|x| println!("{}", x));

  Ok(())
}

```

[`attr`]: https://docs.rs/select/*/select/node/struct.Node.html#method.attr
[`Document::from_read`]: https://docs.rs/select/*/select/document/struct.Document.html#method.from_read
[`filter_map`]: https://doc.rust-lang.org/core/iter/trait.Iterator.html#method.filter_map
[`find`]: https://docs.rs/select/*/select/document/struct.Document.html#method.find
[`Name`]: https://docs.rs/select/*/select/predicate/struct.Name.html
[`reqwest::get`]: https://docs.rs/reqwest/*/reqwest/fn.get.html
[`Selection`]: https://docs.rs/select/*/select/selection/struct.Selection.html
