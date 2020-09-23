## 用 Serde 处理无效的 CSV 数据

<!--
> [encoding/csv/invalid.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/encoding/csv/invalid.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![csv-badge]][csv] [![serde-badge]][serde] [![cat-encoding-badge]][cat-encoding]

CSV 文件通常包含无效数据。对于这些情形，`csv` crate 提供了一个自定义的反序列化程序 [`csv::invalid_option`]，它自动将无效数据转换为 None 值。

```rust,edition2018
use csv::Error;
use serde::Deserialize;

#[derive(Debug, Deserialize)]
struct Record {
    name: String,
    place: String,
    #[serde(deserialize_with = "csv::invalid_option")]
    id: Option<u64>,
}

fn main() -> Result<(), Error> {
    let data = "name,place,id
mark,sydney,46.5
ashley,zurich,92
akshat,delhi,37
alisha,colombo,xyz";

    let mut rdr = csv::Reader::from_reader(data.as_bytes());
    for result in rdr.deserialize() {
        let record: Record = result?;
        println!("{:?}", record);
    }

    Ok(())
}
```

[`csv::invalid_option`]: https://docs.rs/csv/*/csv/fn.invalid_option.html
