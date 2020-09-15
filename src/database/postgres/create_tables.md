## Postgres 数据库中创建表

<!--
> [database/postgres/create_tables.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/database/postgres/create_tables.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![postgres-badge]][postgres] [![cat-database-badge]][cat-database]

Postgres 数据库中，使用 [`postgres`] crate 创建表。

[`Client::connect`] 用于连接到现有数据库。本实例中使用 `Client::connect` 格式化连接数据库的 URL 字符串。假设存在一个数据库：名为 `library`，用户名为 `postgres`，密码为 `postgres`。

```rust,edition2018,no_run
use postgres::{Client, NoTls, Error};

fn main() -> Result<(), Error> {
    let mut client = Client::connect("postgresql://postgres:postgres@localhost/library", NoTls)?;
    
    client.batch_execute("
        CREATE TABLE IF NOT EXISTS author (
            id              SERIAL PRIMARY KEY,
            name            VARCHAR NOT NULL,
            country         VARCHAR NOT NULL
            )
    ")?;

    client.batch_execute("
        CREATE TABLE IF NOT EXISTS book  (
            id              SERIAL PRIMARY KEY,
            title           VARCHAR NOT NULL,
            author_id       INTEGER NOT NULL REFERENCES author
            )
    ")?;

    Ok(())

}
```

[`postgres`]: https://docs.rs/postgres/0.17.2/postgres/
[`Client::connect`]: https://docs.rs/postgres/0.17.2/postgres/struct.Client.html#method.connect
