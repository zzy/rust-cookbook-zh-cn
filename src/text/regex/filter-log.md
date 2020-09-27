## 通过匹配多个正则表达式来筛选日志文件

<!--
> [text/regex/filter-log.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/text/regex/filter-log.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![regex-badge]][regex] [![cat-text-processing-badge]][cat-text-processing]

读取名为 `application.log` 的文件，并且只输出包含下列内容的行：“version X.X.X”、端口为 443 的 IP 地址（如 “192.168.0.1:443”）、特定警告。

正则表达集构造器 [`regex::RegexSetBuilder`] 构建了正则表达式集 [`regex::RegexSet`]。由于反斜杠在正则表达式中非常常见，因此使用[原始字符串字面量][raw string literals]可以使它们更具可读性。

```rust,edition2018,no_run
# use error_chain::error_chain;

use std::fs::File;
use std::io::{BufReader, BufRead};
use regex::RegexSetBuilder;

# error_chain! {
#     foreign_links {
#         Io(std::io::Error);
#         Regex(regex::Error);
#     }
# }
#
fn main() -> Result<()> {
    let log_path = "application.log";
    let buffered = BufReader::new(File::open(log_path)?);

    let set = RegexSetBuilder::new(&[
        r#"version "\d\.\d\.\d""#,
        r#"\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}:443"#,
        r#"warning.*timeout expired"#,
    ]).case_insensitive(true)
        .build()?;

    buffered
        .lines()
        .filter_map(|line| line.ok())
        .filter(|line| set.is_match(line.as_str()))
        .for_each(|x| println!("{}", x));

    Ok(())
}
```

[`regex::RegexSet`]: https://docs.rs/regex/*/regex/struct.RegexSet.html
[`regex::RegexSetBuilder`]: https://docs.rs/regex/*/regex/struct.RegexSetBuilder.html

[raw string literals]: https://rust-reference.budshome.com/tokens.html#%E5%AD%97%E9%9D%A2%E9%87%8F
