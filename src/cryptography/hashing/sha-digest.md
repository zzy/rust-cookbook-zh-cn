## 计算文件的 SHA-256 摘要

<!--
> [cryptography/hashing/sha-digest.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/cryptography/hashing/sha-digest.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![ring-badge]][ring] [![data-encoding-badge]][data-encoding] [![cat-cryptography-badge]][cat-cryptography]

如下实例中，先创建文件，写入一些数据。然后使用 [`digest::Context`] 计算文件内容的 SHA-256 摘要 [`digest::Digest`]。

```rust,edition2018
# use error_chain::error_chain;
use data_encoding::HEXUPPER;
use ring::digest::{Context, Digest, SHA256};
use std::fs::File;
use std::io::{BufReader, Read, Write};
#
# error_chain! {
#     foreign_links {
#         Io(std::io::Error);
#         Decode(data_encoding::DecodeError);
#     }
# }

fn sha256_digest<R: Read>(mut reader: R) -> Result<Digest> {
    let mut context = Context::new(&SHA256);
    let mut buffer = [0; 1024];

    loop {
        let count = reader.read(&mut buffer)?;
        if count == 0 {
            break;
        }
        context.update(&buffer[..count]);
    }

    Ok(context.finish())
}

fn main() -> Result<()> {
    let path = "file.txt";

    let mut output = File::create(path)?;
    write!(output, "We will generate a digest of this text")?;

    let input = File::open(path)?;
    let reader = BufReader::new(input);
    let digest = sha256_digest(reader)?;

    println!("SHA-256 digest is {}", HEXUPPER.encode(digest.as_ref()));

    Ok(())
}
```

[`digest::Context`]: https://briansmith.org/rustdoc/ring/digest/struct.Context.html
[`digest::Digest`]: https://briansmith.org/rustdoc/ring/digest/struct.Digest.html
