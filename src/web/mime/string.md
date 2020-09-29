## 从字符串获取 MIME 类型

<!--
> [web/mime/string.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/mime/string.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![mime-badge]][mime] [![cat-encoding-badge]][cat-encoding]

下面的实例展示如何使用 [mime] crate 从字符串解析出 [`MIME`] 类型。[`FromStrError`] 结构体在 `unwrap_or` 子句中生成默认的 [`MIME`] 类型。

```rust,edition2018
use mime::{Mime, APPLICATION_OCTET_STREAM};

fn main() {
    let invalid_mime_type = "i n v a l i d";
    let default_mime = invalid_mime_type
        .parse::<Mime>()
        .unwrap_or(APPLICATION_OCTET_STREAM);

    println!(
        "MIME for {:?} used default value {:?}",
        invalid_mime_type, default_mime
    );

    let valid_mime_type = "TEXT/PLAIN";
    let parsed_mime = valid_mime_type
        .parse::<Mime>()
        .unwrap_or(APPLICATION_OCTET_STREAM);

    println!(
        "MIME for {:?} was parsed as {:?}",
        valid_mime_type, parsed_mime
    );
}
```

[`FromStrError`]: https://docs.rs/mime/*/mime/struct.FromStrError.html
[`MIME`]: https://docs.rs/mime/*/mime/struct.Mime.html
