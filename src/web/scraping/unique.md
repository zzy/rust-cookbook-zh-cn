## 从 MediaWiki 标记页面提取所有唯一性链接

<!--
> [web/scraping/unique.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/scraping/unique.md)
> <br />
> commit dd4efa8dcd8e611326caa01c08db8f227aa909d6 - 2020.06.07
-->

[![reqwest-badge]][reqwest] [![regex-badge]][regex] [![cat-net-badge]][cat-net]

使用 [`reqwest::get`] 获取 MediaWiki 页面的源代码，然后使用 [`Regex::captures_iter`] 查找内部和外部链接的所有条目。使用智能指针 [`Cow`] 可以提供对借用数据的不可变引用，避免分配过多的[`字符串`][`String`]。

阅读 [MediaWiki 链接语法][MediaWiki link syntax]详细了解。

```rust,edition2018,no_run
use lazy_static::lazy_static;
use regex::Regex;
use std::borrow::Cow;
use std::collections::HashSet;
use std::error::Error;

fn extract_links(content: &str) -> HashSet<Cow<str>> {
  lazy_static! {
    static ref WIKI_REGEX: Regex = Regex::new(
      r"(?x)
                \[\[(?P<internal>[^\[\]|]*)[^\[\]]*\]\]    # internal links
                |
                (url=|URL\||\[)(?P<external>http.*?)[ \|}] # external links
            "
    )
    .unwrap();
  }

  let links: HashSet<_> = WIKI_REGEX
    .captures_iter(content)
    .map(|c| match (c.name("internal"), c.name("external")) {
      (Some(val), None) => Cow::from(val.as_str().to_lowercase()),
      (None, Some(val)) => Cow::from(val.as_str()),
      _ => unreachable!(),
    })
    .collect();

  links
}

#[tokio::main]
async fn main() -> Result<(), Box<dyn Error>> {
  let content = reqwest::get(
    "https://en.wikipedia.org/w/index.php?title=Rust_(programming_language)&action=raw",
  )
  .await?
  .text()
  .await?;

  println!("{:#?}", extract_links(content.as_str()));

  Ok(())
}

```

[`Cow`]: https://doc.rust-lang.org/std/borrow/enum.Cow.html
[`reqwest::get`]: https://docs.rs/reqwest/*/reqwest/fn.get.html
[`Regex::captures_iter`]: https://docs.rs/regex/*/regex/struct.Regex.html#method.captures_iter
[`String`]: https://doc.rust-lang.org/std/string/struct.String.html

[MediaWiki link syntax]: https://www.mediawiki.org/wiki/Help:Links
