## 持续处理子进程的输出

<!--
> [os/external/continuous.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/os/external/continuous.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std] [![cat-os-badge]][cat-os]

在[运行外部命令并处理-stdout](#运行外部命令并处理-stdout) 实例中，直到外部命令 [`Command`] 完成，stdout 的处理才开始。下面的实例调用 [`Stdio::piped`] 创建管道，并在 [`BufReader`] 被更新后立即读取 `stdout`，持续不断地处理。

下面的实例等同于 Unix shell 命令 `journalctl | grep usb`。

```rust,edition2018,no_run
use std::process::{Command, Stdio};
use std::io::{BufRead, BufReader, Error, ErrorKind};

fn main() -> Result<(), Error> {
    let stdout = Command::new("journalctl")
        .stdout(Stdio::piped())
        .spawn()?
        .stdout
        .ok_or_else(|| Error::new(ErrorKind::Other,"Could not capture standard output."))?;

    let reader = BufReader::new(stdout);

    reader
        .lines()
        .filter_map(|line| line.ok())
        .filter(|line| line.find("usb").is_some())
        .for_each(|line| println!("{}", line));

     Ok(())
}
```

[`BufReader`]: https://doc.rust-lang.org/std/io/struct.BufReader.html
[`Command`]: https://doc.rust-lang.org/std/process/struct.Command.html
[`Stdio::piped`]: https://doc.rust-lang.org/std/process/struct.Stdio.html
