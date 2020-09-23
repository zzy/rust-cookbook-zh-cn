## 用 Serde 将记录序列化为 CSV

<!--
> [encoding/csv/serde-serialize.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/encoding/csv/serde-serialize.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![csv-badge]][csv] [![serde-badge]][serde] [![cat-encoding-badge]][cat-encoding]

下面的实例展示如何使用 [serde] crate 将自定义结构体序列化为 CSV 记录。

```rust,edition2018
# use error_chain::error_chain;
use serde::Serialize;
use std::io;
#
# error_chain! {
#    foreign_links {
#        IOError(std::io::Error);
#        CSVError(csv::Error);
#    }
# }

#[derive(Serialize)]
struct Record<'a> {
    name: &'a str,
    place: &'a str,
    id: u64,
}

fn main() -> Result<()> {
    let mut wtr = csv::Writer::from_writer(io::stdout());

    let rec1 = Record { name: "Mark", place: "Melbourne", id: 56};
    let rec2 = Record { name: "Ashley", place: "Sydney", id: 64};
    let rec3 = Record { name: "Akshat", place: "Delhi", id: 98};

    wtr.serialize(rec1)?;
    wtr.serialize(rec2)?;
    wtr.serialize(rec3)?;

    wtr.flush()?;

    Ok(())
}
```
