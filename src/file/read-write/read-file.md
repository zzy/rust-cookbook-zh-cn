## 读取文件的字符串行

<!--
> [file/read-write/read-file.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/file/read-write/read-file.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std] [![cat-filesystem-badge]][cat-filesystem]

我们向文件写入三行信息，然后使用 [`BufRead::lines`] 创建的迭代器 [`Lines`] 读取文件，一次读回一行。[`File`] 模块实现了提供 [`BufReader`] 结构体的 [`Read`] trait。[`File::create`] 打开文件 [`File`] 进行写入，[`File::open`] 则进行读取。

```rust,edition2018
use std::fs::File;
use std::io::{Write, BufReader, BufRead, Error};

fn main() -> Result<(), Error> {
    let path = "lines.txt";

    let mut output = File::create(path)?;
    write!(output, "Rust\n💖\nFun")?;

    let input = File::open(path)?;
    let buffered = BufReader::new(input);

    for line in buffered.lines() {
        println!("{}", line?);
    }

    Ok(())
}
```

[`BufRead::lines`]: https://doc.rust-lang.org/std/io/trait.BufRead.html#method.lines
[`BufRead`]: https://doc.rust-lang.org/std/io/trait.BufRead.html
[`BufReader`]: https://doc.rust-lang.org/std/io/struct.BufReader.html
[`File::create`]: https://doc.rust-lang.org/std/fs/struct.File.html#method.create
[`File::open`]: https://doc.rust-lang.org/std/fs/struct.File.html#method.open
[`File`]: https://doc.rust-lang.org/std/fs/struct.File.html
[`Lines`]: https://doc.rust-lang.org/std/io/struct.Lines.html
[`Read`]: https://doc.rust-lang.org/std/io/trait.Read.html
