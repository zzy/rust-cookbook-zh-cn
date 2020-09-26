## 将子进程的 stdout 和 stderr 重定向到同一个文件

<!--
> [os/external/error-file.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/os/external/error-file.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std] [![cat-os-badge]][cat-os]

生成子进程并将 `stdout` 和 `stderr` 重定向到同一个文件。它遵循与[运行管道传输的外部命令](#运行管道传输的外部命令)相同的思想，但是 [`process::Stdio`] 会将输出写入指定的文件。对 `stdout` 和 `stderr` 而言，[`File::try_clone`] 引用相同的文件句柄。它将确保两个句柄使用相同的光标位置进行写入。

下面的实例等同于运行 Unix shell 命令 `ls
. oops >out.txt 2>&1`。

```rust,edition2018,no_run
use std::fs::File;
use std::io::Error;
use std::process::{Command, Stdio};

fn main() -> Result<(), Error> {
    let outputs = File::create("out.txt")?;
    let errors = outputs.try_clone()?;

    Command::new("ls")
        .args(&[".", "oops"])
        .stdout(Stdio::from(outputs))
        .stderr(Stdio::from(errors))
        .spawn()?
        .wait_with_output()?;

    Ok(())
}
```

[`File::try_clone`]: https://doc.rust-lang.org/std/fs/struct.File.html#method.try_clone
[`process::Stdio`]: https://doc.rust-lang.org/std/process/struct.Stdio.html
