## 对非结构化 JSON 序列化和反序列化

<!--
> [encoding/complex/json.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/encoding/complex/json.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![serde-json-badge]][serde-json] [![cat-encoding-badge]][cat-encoding]

[`serde_json`] crate 提供了 [`from_str`] 函数来解析 JSON 切片 `&str`。

非结构化 JSON 可以被解析为一个通用的 [`serde_json::Value`] 类型，该类型能够表示任何有效的 JSON 数据。

下面的实例展示如何解析 JSON 切片 `&str`，期望值被 [`json!`] 宏声明。

```rust,edition2018
 use serde_json::json;
use serde_json::{Value, Error};

fn main() -> Result<(), Error> {
    let j = r#"{
                 "userid": 103609,
                 "verified": true,
                 "access_privileges": [
                   "user",
                   "admin"
                 ]
               }"#;

    let parsed: Value = serde_json::from_str(j)?;

    let expected = json!({
        "userid": 103609,
        "verified": true,
        "access_privileges": [
            "user",
            "admin"
        ]
    });

    assert_eq!(parsed, expected);

    Ok(())
}
```

[`from_str`]: https://docs.serde.rs/serde_json/fn.from_str.html
[`json!`]: https://docs.serde.rs/serde_json/macro.json.html
[`serde_json`]: https://docs.serde.rs/serde_json/
[`serde_json::Value`]: https://docs.serde.rs/serde_json/enum.Value.html
