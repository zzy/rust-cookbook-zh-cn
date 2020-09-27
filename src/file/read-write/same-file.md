## 避免读取写入同一文件

<!--
> [file/read-write/same-file.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/file/read-write/same-file.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![same_file-badge]][same_file] [![cat-filesystem-badge]][cat-filesystem]

对文件使用 [`same_file::Handle`] 结构体，可以测试文件句柄是否等同。在本实例中，将对要读取和写入的文件句柄进行相等性测试。

```rust,edition2018,no_run
use same_file::Handle;
use std::fs::File;
use std::io::{BufRead, BufReader, Error, ErrorKind};
use std::path::Path;

fn main() -> Result<(), Error> {
    let path_to_read = Path::new("new.txt");

    let stdout_handle = Handle::stdout()?;
    let handle = Handle::from_path(path_to_read)?;

    if stdout_handle == handle {
        return Err(Error::new(
            ErrorKind::Other,
            "You are reading and writing to the same file",
        ));
    } else {
        let file = File::open(&path_to_read)?;
        let file = BufReader::new(file);
        for (num, line) in file.lines().enumerate() {
            println!("{} : {}", num, line?.to_uppercase());
        }
    }

    Ok(())
}
```

```bash
cargo run
```
显示文件 new.txt 的内容。

```bash
cargo run >> ./new.txt
```
报错，因为是同一文件。

[`same_file::Handle`]: https://docs.rs/same-file/*/same_file/struct.Handle.html
