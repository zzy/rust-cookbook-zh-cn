## 忽略文件名大小写，使用给定模式查找所有文件

<!--
> [file/dir/ignore-case.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/file/dir/ignore-case.md)
> <br />
> commit 203b1085212a7b857d9a29bdc6a763515e77e0f9 - 2020.06.08
-->

[![glob-badge]][glob] [![cat-filesystem-badge]][cat-filesystem]

在 `/media/` 目录中查找与正则表达模式 `img_[0-9]*.png` 匹配的所有图像文件。

一个自定义 [`MatchOptions`] 结构体被传递给 [`glob_with`] 函数，使全局命令模式下不区分大小写，同时保持其他选项的默认值 [`Default`]。

> 译者注：`glob` 是 `glob command` 的简写。在 shell 里面，用 `*` 等匹配模式来匹配文件，如：ls src/*.rs。

```rust,edition2018,no_run
use error_chain::error_chain;
use glob::{glob_with, MatchOptions};

error_chain! {
    foreign_links {
        Glob(glob::GlobError);
        Pattern(glob::PatternError);
    }
}

fn main() -> Result<()> {
    let options = MatchOptions {
        case_sensitive: false,
        ..Default::default()
    };

    for entry in glob_with("/media/img_[0-9]*.png", options)? {
        println!("{}", entry?.display());
    }

    Ok(())
}
```

[`Default`]: https://doc.rust-lang.org/std/default/trait.Default.html
[`glob_with`]: https://docs.rs/glob/*/glob/fn.glob_with.html
[`MatchOptions`]: https://docs.rs/glob/*/glob/struct.MatchOptions.html
