## 递归查找所有 png 文件

<!--
> [file/dir/png.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/file/dir/png.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![glob-badge]][glob] [![cat-filesystem-badge]][cat-filesystem]

递归地查找当前目录中的所有 PNG 文件。在本例中，`**` 模式用于匹配当前目录及其所有子目录。

在路径任意部分使用 `**` 模式，例如，`/media/**/*.png` 匹配 `media` 及其子目录中的所有 PNG 文件。

```rust,edition2018,no_run
# use error_chain::error_chain;

use glob::glob;
#
# error_chain! {
#     foreign_links {
#         Glob(glob::GlobError);
#         Pattern(glob::PatternError);
#     }
# }

fn main() -> Result<()> {
    for entry in glob("**/*.png")? {
        println!("{}", entry?.display());
    }

    Ok(())
}
```
