## 使用给定断言递归查找所有文件

<!--
> [file/dir/find-file.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/file/dir/find-file.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![walkdir-badge]][walkdir] [![cat-filesystem-badge]][cat-filesystem]

在当前目录中查找最近一天内修改的 JSON 文件。使用 [`follow_links`] 确保软链接（符号链接）像普通目录和文件一样被按照当前查找规则执行。

```rust,edition2018,no_run
# use error_chain::error_chain;

use walkdir::WalkDir;
#
# error_chain! {
#     foreign_links {
#         WalkDir(walkdir::Error);
#         Io(std::io::Error);
#         SystemTime(std::time::SystemTimeError);
#     }
# }

fn main() -> Result<()> {
    for entry in WalkDir::new(".")
            .follow_links(true)
            .into_iter()
            .filter_map(|e| e.ok()) {
        let f_name = entry.file_name().to_string_lossy();
        let sec = entry.metadata()?.modified()?;

        if f_name.ends_with(".json") && sec.elapsed()?.as_secs() < 86400 {
            println!("{}", f_name);
        }
    }

    Ok(())
}
```

[`follow_links`]: https://docs.rs/walkdir/*/walkdir/struct.WalkDir.html#method.follow_links
