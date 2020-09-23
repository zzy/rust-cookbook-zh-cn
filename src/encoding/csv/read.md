## 读取 CSV 记录

<!--
> [encoding/csv/read.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/encoding/csv/read.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![csv-badge]][csv] [![cat-encoding-badge]][cat-encoding]

将标准的 CSV 记录读入 [`csv::StringRecord`]——一种弱类型的数据表示方式，它需要 CSV 中的行数据是有效的 UTF-8 字符编码。另外，[`csv::ByteRecord`] 对 UTF-8 不做任何预设。

```rust,edition2018
use csv::Error;

fn main() -> Result<(), Error> {
    let csv = "year,make,model,description
		1948,Porsche,356,Luxury sports car
		1967,Ford,Mustang fastback 1967,American car";

    let mut reader = csv::Reader::from_reader(csv.as_bytes());
    for record in reader.records() {
        let record = record?;
        println!(
            "In {}, {} built the {} model. It is a {}.",
            &record[0],
            &record[1],
            &record[2],
            &record[3]
        );
    }

    Ok(())
}
```

Serde 将数据反序列化为强类型结构体。具体查阅 [`csv::Reader::deserialize`] 方法。

```rust,edition2018
use serde::Deserialize;
#[derive(Deserialize)]
struct Record {
    year: u16,
    make: String,
    model: String,
    description: String,
}

fn main() -> Result<(), csv::Error> {
    let csv = "year,make,model,description
1948,Porsche,356,Luxury sports car
1967,Ford,Mustang fastback 1967,American car";

    let mut reader = csv::Reader::from_reader(csv.as_bytes());

    for record in reader.deserialize() {
        let record: Record = record?;
        println!(
            "In {}, {} built the {} model. It is a {}.",
            record.year,
            record.make,
            record.model,
            record.description
        );
    }

    Ok(())
}
```

[`csv::ByteRecord`]: https://docs.rs/csv/*/csv/struct.ByteRecord.html
[`csv::Reader::deserialize`]: https://docs.rs/csv/*/csv/struct.Reader.html#method.deserialize
[`csv::StringRecord`]: https://docs.rs/csv/*/csv/struct.StringRecord.html
