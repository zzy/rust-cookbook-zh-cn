## 数据聚合

<!--
> [database/postgres/aggregate_data.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/database/postgres/aggregate_data.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![postgres-badge]][postgres] [![cat-database-badge]][cat-database]

下述实例按照降序列出了[`美国纽约州现代艺术博物馆`][`Museum of Modern Art`]数据库中首批 7999 位艺术家的国籍。

```rust,edition2018,no_run
use postgres::{Client, Error, NoTls};

struct Nation {
    nationality: String,
    count: i64,
}

fn main() -> Result<(), Error> {
    let mut client = Client::connect(
        "postgresql://postgres:postgres@127.0.0.1/moma",
        NoTls,
    )?;

    for row in client.query 
	("SELECT nationality, COUNT(nationality) AS count 
	FROM artists GROUP BY nationality ORDER BY count DESC", &[])? {
        
        let (nationality, count) : (Option<String>, Option<i64>) 
		= (row.get (0), row.get (1));
        
        if nationality.is_some () && count.is_some () {

            let nation = Nation{
                nationality: nationality.unwrap(),
                count: count.unwrap(),
        };
            println!("{} {}", nation.nationality, nation.count);
            
        }
    }

    Ok(())
}
```

[`Museum of Modern Art`]: https://github.com/MuseumofModernArt/collection/blob/master/Artists.csv
