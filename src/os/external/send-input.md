## 运行传递 stdin 的外部命令，并检查错误代码

<!--
> [os/external/send-input.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/os/external/send-input.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std] [![cat-os-badge]][cat-os]

使用外部命令 [`Command`] 打开 `python` 解释器，并传递一条 python 语句供其执行，然后解析语句的输出结构体 [`Output`]。

```rust,edition2018,no_run
# use error_chain::error_chain;
#
use std::collections::HashSet;
use std::io::Write;
use std::process::{Command, Stdio};
#
# error_chain!{
#     errors { CmdError }
#     foreign_links {
#         Io(std::io::Error);
#         Utf8(std::string::FromUtf8Error);
#     }
# }

fn main() -> Result<()> {
    let mut child = Command::new("python").stdin(Stdio::piped())
        .stderr(Stdio::piped())
        .stdout(Stdio::piped())
        .spawn()?;

    child.stdin
        .as_mut()
        .ok_or("Child process stdin has not been captured!")?
        .write_all(b"import this; copyright(); credits(); exit()")?;

    let output = child.wait_with_output()?;

    if output.status.success() {
        let raw_output = String::from_utf8(output.stdout)?;
        let words = raw_output.split_whitespace()
            .map(|s| s.to_lowercase())
            .collect::<HashSet<_>>();
        println!("Found {} unique words:", words.len());
        println!("{:#?}", words);
        Ok(())
    } else {
        let err = String::from_utf8(output.stderr)?;
        error_chain::bail!("External command failed:\n {}", err)
    }
}
```

[`Command`]: https://doc.rust-lang.org/std/process/struct.Command.html
[`Output`]: https://doc.rust-lang.org/std/process/struct.Output.html
