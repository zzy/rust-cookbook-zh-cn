## 编码和解码 base64

<!--
> [encoding/string/base64.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/encoding/string/base64.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![base64-badge]][base64] [![cat-encoding-badge]][cat-encoding]

使用 [`encode`] 将字节切片编码为 `base64` 字符串，对 `base64` 字符串解码使用 [`decode`]。

```rust,edition2018
# use error_chain::error_chain;

use std::str;
use base64::{encode, decode};
#
# error_chain! {
#     foreign_links {
#         Base64(base64::DecodeError);
#         Utf8Error(str::Utf8Error);
#     }
# }

fn main() -> Result<()> {
    let hello = b"hello rustaceans";
    let encoded = encode(hello);
    let decoded = decode(&encoded)?;

    println!("origin: {}", str::from_utf8(hello)?);
    println!("base64 encoded: {}", encoded);
    println!("back to origin: {}", str::from_utf8(&decoded)?);

    Ok(())
}
```

[`decode`]: https://docs.rs/base64/*/base64/fn.decode.html
[`encode`]: https://docs.rs/base64/*/base64/fn.encode.html
