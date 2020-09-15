## 创建 SQLite 数据库

<!--
> [database/sqlite/initialization.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/database/sqlite/initialization.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![rusqlite-badge]][rusqlite] [![cat-database-badge]][cat-database]

使用 `rusqlite` crate 打开 SQLite 数据库连接。Windows 上编译 `rusqlite` crate 请参考[文档][documentation]。

如果数据库不存在，[`Connection::open`] 方法将创建它。

```rust,edition2018,no_run
use rusqlite::{Connection, Result};
use rusqlite::NO_PARAMS;

fn main() -> Result<()> {
    let conn = Connection::open("cats.db")?;

    conn.execute(
        "create table if not exists cat_colors (
             id integer primary key,
             name text not null unique
         )",
        NO_PARAMS,
    )?;
    conn.execute(
        "create table if not exists cats (
             id integer primary key,
             name text not null,
             color_id integer not null references cat_colors(id)
         )",
        NO_PARAMS,
    )?;

    Ok(())
}
```

[`Connection::open`]: https://docs.rs/rusqlite/*/rusqlite/struct.Connection.html#method.open

[documentation]: https://github.com/jgallagher/rusqlite#user-content-notes-on-building-rusqlite-and-libsqlite3-sys
