## 验证并提取电子邮件登录信息

<!--
> [text/regex/email.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/text/regex/email.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![regex-badge]][regex] [![lazy_static-badge]][lazy_static] [![cat-text-processing-badge]][cat-text-processing]

验证电子邮件地址的格式是否正确，并提取 @ 符号之前的所有内容。

```rust,edition2018
use lazy_static::lazy_static;

use regex::Regex;

fn extract_login(input: &str) -> Option<&str> {
    lazy_static! {
        static ref RE: Regex = Regex::new(r"(?x)
            ^(?P<login>[^@\s]+)@
            ([[:word:]]+\.)*
            [[:word:]]+$
            ").unwrap();
    }
    RE.captures(input).and_then(|cap| {
        cap.name("login").map(|login| login.as_str())
    })
}

fn main() {
    assert_eq!(extract_login(r"I❤email@example.com"), Some(r"I❤email"));
    assert_eq!(
        extract_login(r"sdf+sdsfsd.as.sdsd@jhkk.d.rl"),
        Some(r"sdf+sdsfsd.as.sdsd")
    );
    assert_eq!(extract_login(r"More@Than@One@at.com"), None);
    assert_eq!(extract_login(r"Not an email@email"), None);
}
```
