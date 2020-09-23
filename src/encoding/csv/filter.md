## 筛选匹配断言的 CSV 记录

<!--
> [encoding/csv/filter.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/encoding/csv/filter.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![csv-badge]][csv] [![cat-encoding-badge]][cat-encoding]

_仅仅_ 返回 `data` 中字段（field）与 `query` 匹配的的行。

```rust,edition2018
# use error_chain::error_chain;

use std::io;
#
# error_chain!{
#     foreign_links {
#         Io(std::io::Error);
#         CsvError(csv::Error);
#     }
# }

fn main() -> Result<()> {
    let query = "CA";
    let data = "\
City,State,Population,Latitude,Longitude
Kenai,AK,7610,60.5544444,-151.2583333
Oakman,AL,,33.7133333,-87.3886111
Sandfort,AL,,32.3380556,-85.2233333
West Hollywood,CA,37031,34.0900000,-118.3608333";

    let mut rdr = csv::ReaderBuilder::new().from_reader(data.as_bytes());
    let mut wtr = csv::Writer::from_writer(io::stdout());

    wtr.write_record(rdr.headers()?)?;

    for result in rdr.records() {
        let record = result?;
        if record.iter().any(|field| field == query) {
            wtr.write_record(&record)?;
        }
    }

    wtr.flush()?;
    Ok(())
}
```

_免责声明：此实例改编自[csv crate 教程](https://docs.rs/csv/*/csv/tutorial/index.html#filter-by-search)_。
