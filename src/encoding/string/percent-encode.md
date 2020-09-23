## 百分比编码（URL 编码）字符串

<!--
> [encoding/string/percent-encode.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/encoding/string/percent-encode.md)
> <br />
> commit 203b1085212a7b857d9a29bdc6a763515e77e0f9 - 2020.06.08
-->

[![percent-encoding-badge]][percent-encoding] [![cat-encoding-badge]][cat-encoding]

使用 `percent-encoding` crate 中的 [`utf8_percent_encode`] 函数对输入字符串进行[百分比编码（URL 编码）][percent-encoding]。解码使用 [`percent_decode`] 函数。

```rust,edition2018
use percent_encoding::{utf8_percent_encode, percent_decode, AsciiSet, CONTROLS};
use std::str::Utf8Error;

/// https://url.spec.whatwg.org/#fragment-percent-encode-set
const FRAGMENT: &AsciiSet = &CONTROLS.add(b' ').add(b'"').add(b'<').add(b'>').add(b'`');

fn main() -> Result<(), Utf8Error> {
    let input = "confident, productive systems programming";

    let iter = utf8_percent_encode(input, FRAGMENT);
    let encoded: String = iter.collect();
    assert_eq!(encoded, "confident,%20productive%20systems%20programming");

    let iter = percent_decode(encoded.as_bytes());
    let decoded = iter.decode_utf8()?;
    assert_eq!(decoded, "confident, productive systems programming");

    Ok(())
}
```

编码集定义哪些字节（除了非 ASCII 字节和控制键之外）需要进行百分比编码（URL 编码），这个集合的选择取决于上下文。例如，`url` 对 URL 路径中的 `?` 编码，而不对查询字符串中的 `?` 编码。

编码的返回值是 `&str` 切片的迭代器，然后聚集为一个字符串 `String`。

[`percent_decode`]: https://docs.rs/percent-encoding/*/percent_encoding/fn.percent_decode.html
[`utf8_percent_encode`]: https://docs.rs/percent-encoding/*/percent_encoding/fn.utf8_percent_encode.html

[percent-encoding]: https://en.wikipedia.org/wiki/Percent-encoding
