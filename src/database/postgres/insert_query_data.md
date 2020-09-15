## 数据插入和查询

<!--
> [database/postgres/insert_query_data.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/database/postgres/insert_query_data.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![postgres-badge]][postgres] [![cat-database-badge]][cat-database]

下述实例中使用 `Client` 的 [`execute`] 方法将数据插入到 `author` 表中。然后，使用 `Client` 的 [`query`] 方法查询 `author` 表中的数据。

```rust,edition2018,no_run
use postgres::{Client, NoTls, Error};
use std::collections::HashMap;

struct Author {
    _id: i32,
    name: String,
    country: String
}

fn main() -> Result<(), Error> {
    let mut client = Client::connect("postgresql://postgres:postgres@localhost/library", 
                                    NoTls)?;
    
    let mut authors = HashMap::new();
    authors.insert(String::from("Chinua Achebe"), "Nigeria");
    authors.insert(String::from("Rabindranath Tagore"), "India");
    authors.insert(String::from("Anita Nair"), "India");

    for (key, value) in &authors {
        let author = Author {
            _id: 0,
            name: key.to_string(),
            country: value.to_string()
        };

        client.execute(
                "INSERT INTO author (name, country) VALUES ($1, $2)",
                &[&author.name, &author.country],
        )?;
    }

    for row in client.query("SELECT id, name, country FROM author", &[])? {
        let author = Author {
            _id: row.get(0),
            name: row.get(1),
            country: row.get(2),
        };
        println!("Author {} is from {}", author.name, author.country);
    }

    Ok(())

}
```

[`execute`]: https://docs.rs/postgres/0.17.2/postgres/struct.Client.html#method.execute
[`query`]: https://docs.rs/postgres/0.17.2/postgres/struct.Client.html#method.query
