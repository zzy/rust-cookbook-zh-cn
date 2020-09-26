## 查找给定路径的循环

<!--
> [file/dir/loops.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/file/dir/loops.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![same_file-badge]][same_file] [![cat-filesystem-badge]][cat-filesystem]

使用 [`same_file::is_same_file`] 检测给定路径的循环。例如，可以通过软连接（符号链接）在 Unix 系统上创建循环：

```bash
mkdir -p /tmp/foo/bar/baz
ln -s /tmp/foo/  /tmp/foo/bar/baz/qux
```

下面的实例将断言存在一个循环。

```rust,edition2018,no_run
use std::io;
use std::path::{Path, PathBuf};
use same_file::is_same_file;

fn contains_loop<P: AsRef<Path>>(path: P) -> io::Result<Option<(PathBuf, PathBuf)>> {
    let path = path.as_ref();
    let mut path_buf = path.to_path_buf();
    while path_buf.pop() {
        if is_same_file(&path_buf, path)? {
            return Ok(Some((path_buf, path.to_path_buf())));
        } else if let Some(looped_paths) = contains_loop(&path_buf)? {
            return Ok(Some(looped_paths));
        }
    }
    return Ok(None);
}

fn main() {
    assert_eq!(
        contains_loop("/tmp/foo/bar/baz/qux/bar/baz").unwrap(),
        Some((
            PathBuf::from("/tmp/foo"),
            PathBuf::from("/tmp/foo/bar/baz/qux")
        ))
    );
}
```

[`same_file::is_same_file`]: https://docs.rs/same-file/*/same_file/fn.is_same_file.html
