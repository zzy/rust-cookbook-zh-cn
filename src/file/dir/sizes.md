## 在给定深度的目录，递归计算文件大小

<!--
> [file/dir/sizes.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/file/dir/sizes.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![walkdir-badge]][walkdir] [![cat-filesystem-badge]][cat-filesystem]

通过 [`WalkDir::min_depth`] 和 [`WalkDir::max_depth`] 方法，可以灵活设置目录的递归深度。下面的实例计算了 3 层子文件夹深度的所有文件的大小总和，计算中忽略根文件夹中的文件。

```rust,edition2018
use walkdir::WalkDir;

fn main() {
    let total_size = WalkDir::new(".")
        .min_depth(1)
        .max_depth(3)
        .into_iter()
        .filter_map(|entry| entry.ok())
        .filter_map(|entry| entry.metadata().ok())
        .filter(|metadata| metadata.is_file())
        .fold(0, |acc, m| acc + m.len());

    println!("Total size: {} bytes.", total_size);
}
```

[`WalkDir::max_depth`]: https://docs.rs/walkdir/*/walkdir/struct.WalkDir.html#method.max_depth
[`WalkDir::min_depth`]: https://docs.rs/walkdir/*/walkdir/struct.WalkDir.html#method.min_depth
