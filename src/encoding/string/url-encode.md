## 将字符串编码为 application/x-www-form-urlencoded

<!--
> [encoding/string/hex.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/encoding/string/hex.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![url-badge]][url] [![cat-encoding-badge]][cat-encoding]

如下实例使用 [`form_urlencoded::byte_serialize`] 将字符串编码为 [application/x-www-form-urlencoded] 表单语法，随后使用 [`form_urlencoded::parse`] 对其进行解码。这两个函数都返回迭代器，然后这些迭代器聚集为 `String`。

```rust,edition2018
use url::form_urlencoded::{byte_serialize, parse};

fn main() {
    let urlencoded: String = byte_serialize("What is ❤?".as_bytes()).collect();
    assert_eq!(urlencoded, "What+is+%E2%9D%A4%3F");
    println!("urlencoded:'{}'", urlencoded);

    let decoded: String = parse(urlencoded.as_bytes())
        .map(|(key, val)| [key, val].concat())
        .collect();
    assert_eq!(decoded, "What is ❤?");
    println!("decoded:'{}'", decoded);
}
```

[`form_urlencoded::byte_serialize`]: https://docs.rs/url/*/url/form_urlencoded/fn.byte_serialize.html
[`form_urlencoded::parse`]: https://docs.rs/url/*/url/form_urlencoded/fn.parse.html

[application/x-www-form-urlencoded]: https://url.spec.whatwg.org/#application/x-www-form-urlencoded
