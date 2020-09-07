## 从路径移除前缀时，解压 tar 包

<!--
> [compression/tar/tar-strip-prefix.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/compression/tar/tar-strip-prefix.md)
> <br />
> commit - b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![flate2-badge]][flate2] [![tar-badge]][tar] [![cat-compression-badge]][cat-compression]

循环遍历 [`Archive::entries`]。使用 [`Path::strip_prefix`] 移除指定的路径前缀（`bundle/logs`）。最终，通过 [`Entry::unpack`] 提取 [`tar::Entry`]（tar 包中的内容）。

```rust,edition2018,no_run
# use error_chain::error_chain;
use std::fs::File;
use std::path::PathBuf;
use flate2::read::GzDecoder;
use tar::Archive;
# 
# error_chain! {
#   foreign_links {
#     Io(std::io::Error);
#     StripPrefixError(::std::path::StripPrefixError);
#   }
# }

fn main() -> Result<()> {
    let file = File::open("archive.tar.gz")?;
    let mut archive = Archive::new(GzDecoder::new(file));
    let prefix = "bundle/logs";

    println!("Extracted the following files:");
    archive
        .entries()?
        .filter_map(|e| e.ok())
        .map(|mut entry| -> Result<PathBuf> {
            let path = entry.path()?.strip_prefix(prefix)?.to_owned();
            entry.unpack(&path)?;
            Ok(path)
        })
        .filter_map(|e| e.ok())
        .for_each(|x| println!("> {}", x.display()));

    Ok(())
}
```

[`Archive::entries`]: https://docs.rs/tar/*/tar/struct.Archive.html#method.entries
[`Entry::unpack`]: https://docs.rs/tar/*/tar/struct.Entry.html#method.unpack
[`Path::strip_prefix`]: https://doc.rust-lang.org/std/path/struct.Path.html#method.strip_prefix
[`tar::Entry`]: https://docs.rs/tar/*/tar/struct.Entry.html
