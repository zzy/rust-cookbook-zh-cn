## 编码和解码十六进制

<!--
> [encoding/string/hex.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/encoding/string/hex.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![data-encoding-badge]][data-encoding] [![cat-encoding-badge]][cat-encoding]

[`data_encoding`] crate 提供了 `HEXUPPER::encode` 方法，该方法接受 `&[u8]` 参数并返回十六进制数据的字符串 `String`。

类似地，[`data_encoding`] crate 提供了 `HEXUPPER::decode` 方法，该方法接受 `&[u8]` 参数。如果输入数据被成功解码，则返回 `Vec<u8>`。

下面的实例将 `&[u8]` 数据转换为等效的十六进制数据，然后将此值与预期值进行比较。

```rust,edition2018
use data_encoding::{HEXUPPER, DecodeError};

fn main() -> Result<(), DecodeError> {
    let original = b"The quick brown fox jumps over the lazy dog.";
    let expected = "54686520717569636B2062726F776E20666F78206A756D7073206F76\
        657220746865206C617A7920646F672E";

    let encoded = HEXUPPER.encode(original);
    assert_eq!(encoded, expected);

    let decoded = HEXUPPER.decode(&encoded.into_bytes())?;
    assert_eq!(&decoded[..], &original[..]);

    Ok(())
}
```

[`data_encoding`]: https://docs.rs/data-encoding/*/data_encoding/
