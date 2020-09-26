## 使用内存映射随机访问文件

<!--
> [file/read-write/memmap.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/file/read-write/memmap.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![memmap-badge]][memmap] [![cat-filesystem-badge]][cat-filesystem]

使用 [memmap] 创建文件的内存映射，并模拟文件的一些非序列读取。使用内存映射意味着您仅需索引一个切片，而不是使用 [`seek`] 方法来导航整个文件。

[`Mmap::map`] 函数假定内存映射后的文件没有被另一个进程同时更改，否则会出现[竞态条件][race condition]。

```rust,edition2018
use memmap::Mmap;
use std::fs::File;
use std::io::{Write, Error};

fn main() -> Result<(), Error> {
#     write!(File::create("content.txt")?, "My hovercraft is full of eels!")?;
#
    let file = File::open("content.txt")?;
    let map = unsafe { Mmap::map(&file)? };

    let random_indexes = [0, 1, 2, 19, 22, 10, 11, 29];
    assert_eq!(&map[3..13], b"hovercraft");
    let random_bytes: Vec<u8> = random_indexes.iter()
        .map(|&idx| map[idx])
        .collect();
    assert_eq!(&random_bytes[..], b"My loaf!");
    Ok(())
}
```

[`Mmap::map`]: https://docs.rs/memmap/*/memmap/struct.Mmap.html#method.map
[`seek`]: https://doc.rust-lang.org/std/fs/struct.File.html#method.seek

[race condition]: https://baike.baidu.com/item/%E7%AB%9E%E4%BA%89%E5%8D%B1%E5%AE%B3/3525767
