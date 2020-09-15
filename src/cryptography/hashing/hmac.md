## 使用 HMAC 摘要对消息进行签名和验证

<!--
> [cryptography/hashing/hmac.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/cryptography/hashing/hmac.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![ring-badge]][ring] [![cat-cryptography-badge]][cat-cryptography]

使用 [`ring::hmac`] 创建字符串的签名 [`hmac::Signature`]，然后验证签名是否正确。

```rust,edition2018
use ring::{hmac, rand};
use ring::rand::SecureRandom;
use ring::error::Unspecified;

fn main() -> Result<(), Unspecified> {
    let mut key_value = [0u8; 48];
    let rng = rand::SystemRandom::new();
    rng.fill(&mut key_value)?;
    let key = hmac::Key::new(hmac::HMAC_SHA256, &key_value);

    let message = "Legitimate and important message.";
    let signature = hmac::sign(&key, message.as_bytes());
    hmac::verify(&key, message.as_bytes(), signature.as_ref())?;

    Ok(())
}
```

[`hmac::Signature`]: https://briansmith.org/rustdoc/ring/hmac/struct.Signature.html
[`ring::hmac`]: https://briansmith.org/rustdoc/ring/hmac/
