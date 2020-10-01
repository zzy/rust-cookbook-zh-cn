## 文本模式替换

<!--
> [text/regex/replace.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/text/regex/replace.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![regex-badge]][regex] [![lazy_static-badge]][lazy_static] [![cat-text-processing-badge]][cat-text-processing]

将所有出现的国际标准 ISO 8601 日期模式 *YYYY-MM-DD* 替换为具有斜杠的等效美式英语日期模式。例如： `2013-01-15` 替换为 `01/15/2013`。

[`Regex::replace_all`] 方法将替换整个正则表示匹配的所有内容。`&str` 实现了 `Replacer` trait，它允许类似 `$abcde` 的变量引用相应的搜索匹配模式（search regex）中的命名捕获组 `(?P<abcde>REGEX)`。有关示例和转义的详细信息，请参阅[替换字符串语法][replacement string syntax]。

> 译者注：正则表达式的使用，需要了解匹配规则：全文匹配（match regex）、搜索匹配（search regex）、替换匹配（replace regex）。

```rust,edition2018
use lazy_static::lazy_static;

use std::borrow::Cow;
use regex::Regex;

fn reformat_dates(before: &str) -> Cow<str> {
    lazy_static! {
        static ref ISO8601_DATE_REGEX : Regex = Regex::new(
            r"(?P<y>\d{4})-(?P<m>\d{2})-(?P<d>\d{2})"
            ).unwrap();
    }
    ISO8601_DATE_REGEX.replace_all(before, "$m/$d/$y")
}

fn main() {
    let before = "2012-03-14, 2013-01-15 and 2014-07-05";
    let after = reformat_dates(before);
    assert_eq!(after, "03/14/2012, 01/15/2013 and 07/05/2014");
}
```

[`Regex::replace_all`]: https://docs.rs/regex/*/regex/struct.Regex.html#method.replace_all

[replacement string syntax]: https://docs.rs/regex/*/regex/struct.Regex.html#replacement-string-syntax
