## 解压 tar 包

<!--
> [compression/tar/tar-decompress.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/compression/tar/tar-decompress.md)
> <br />
> commit - b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![flate2-badge]][flate2] [![tar-badge]][tar] [![cat-compression-badge]][cat-compression]

从当前工作目录中的压缩包 `archive.tar.gz`，解压（[`GzDecoder`]）和提取（[`Archive::unpack`]）所有文件，并放在同一位置。

```rust,edition2018,no_run

use std::fs::File;
use flate2::read::GzDecoder;
use tar::Archive;

fn main() -> Result<(), std::io::Error> {
    let path = "archive.tar.gz";

    let tar_gz = File::open(path)?;
    let tar = GzDecoder::new(tar_gz);
    let mut archive = Archive::new(tar);
    archive.unpack(".")?;

    Ok(())
}
```

[`Archive::unpack`]: https://docs.rs/tar/*/tar/struct.Archive.html#method.unpack
[`GzDecoder`]: https://docs.rs/flate2/*/flate2/read/struct.GzDecoder.html
