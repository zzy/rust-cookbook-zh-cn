## 压缩目录为 tar 包

<!--
> [compression/tar/tar-compress.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/compression/tar/tar-compress.md)
> <br />
> commit - b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![flate2-badge]][flate2] [![tar-badge]][tar] [![cat-compression-badge]][cat-compression]

压缩 `/var/log` 目录内的内容到 `archive.tar.gz` 压缩包中。

创建一个用 [`GzEncoder`] 和 [`tar::Builder`] 包裹的 [`File`]。

使用 [`Builder::append_dir_all`]，将 `/var/log` 目录内的内容递归添加到 `backup/logs` 路径下的归档文件中。在将数据写入压缩包 `archive.tar.gz` 之前，[`GzEncoder`] 负责清晰地将数据压缩。

```rust,edition2018,no_run

use std::fs::File;
use flate2::Compression;
use flate2::write::GzEncoder;

fn main() -> Result<(), std::io::Error> {
    let tar_gz = File::create("archive.tar.gz")?;
    let enc = GzEncoder::new(tar_gz, Compression::default());
    let mut tar = tar::Builder::new(enc);
    tar.append_dir_all("backup/logs", "/var/log")?;
    Ok(())
}
```

[`Builder::append_dir_all`]: https://docs.rs/tar/*/tar/struct.Builder.html#method.append_dir_all
[`File`]: https://doc.rust-lang.org/std/fs/struct.File.html
[`GzEncoder`]: https://docs.rs/flate2/*/flate2/write/struct.GzEncoder.html
[`tar::Builder`]: https://docs.rs/tar/*/tar/struct.Builder.html
