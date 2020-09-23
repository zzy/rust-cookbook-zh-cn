## 将记录序列化为 CSV

<!--
> [encoding/csv/serialize.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/encoding/csv/serialize.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![csv-badge]][csv] [![cat-encoding-badge]][cat-encoding]

本实例展示了如何序列化 Rust 元组。[`csv::writer`] 支持从 Rust 类型到 CSV 记录的自动序列化。[`write_record`] 只写入包含字符串数据的简单记录。具有更复杂值（如数字、浮点和选项）的数据使用 [`serialize`] 进行序列化。因为 [`csv::writer`] 使用内部缓冲区，所以在完成时总是显式刷新 [`flush`]。

```rust,edition2018
# use error_chain::error_chain;

use std::io;
#
# error_chain! {
#     foreign_links {
#         CSVError(csv::Error);
#         IOError(std::io::Error);
#    }
# }

fn main() -> Result<()> {
    let mut wtr = csv::Writer::from_writer(io::stdout());

    wtr.write_record(&["Name", "Place", "ID"])?;

    wtr.serialize(("Mark", "Sydney", 87))?;
    wtr.serialize(("Ashley", "Dublin", 32))?;
    wtr.serialize(("Akshat", "Delhi", 11))?;

    wtr.flush()?;
    Ok(())
}
```

[`csv::Writer`]: https://docs.rs/csv/*/csv/struct.Writer.html
[`flush`]: https://docs.rs/csv/*/csv/struct.Writer.html#method.flush
[`serialize`]: https://docs.rs/csv/*/csv/struct.Writer.html#method.serialize
[`write_record`]: https://docs.rs/csv/*/csv/struct.Writer.html#method.write_record
