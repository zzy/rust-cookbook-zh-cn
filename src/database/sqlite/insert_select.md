## 数据插入和查询

<!--
> [database/sqlite/insert_select.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/database/sqlite/insert_select.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![rusqlite-badge]][rusqlite] [![cat-database-badge]][cat-database]

[`Connection::open`] 将打开在前一章节实例中创建的数据库 `cats` 的连接。下面的实例使用 `Connection` 的 [`execute`] 方法将数据插入 `cat_colors` 和 `cats` 两张表中。首先，将数据插入到 `cat_colors` 表中。随后，使用 `Connection` 的 [`last_insert_rowid`] 方法来获取 `cat_colors` 表最后插入记录的 `id`。当向 `cats` 表中插入数据时，使用此 `id`。然后，使用 [`prepare`] 方法准备执行 select 查询操作，该方法提供 [`statement`] 结构体。最后，使用 [`statement`] 的 [`query_map`] 方法执行查询。

```rust,no_run

use rusqlite::NO_PARAMS;
use rusqlite::{Connection, Result};
use std::collections::HashMap;

#[derive(Debug)]
struct Cat {
    name: String,
    color: String,
}

fn main() -> Result<()> {
    let conn = Connection::open("cats.db")?;

    let mut cat_colors = HashMap::new();
    cat_colors.insert(String::from("Blue"), vec!["Tigger", "Sammy"]);
    cat_colors.insert(String::from("Black"), vec!["Oreo", "Biscuit"]);

    for (color, catnames) in &cat_colors {
        conn.execute(
            "INSERT INTO cat_colors (name) values (?1)",
            &[&color.to_string()],
        )?;
        let last_id: String = conn.last_insert_rowid().to_string();

        for cat in catnames {
            conn.execute(
                "INSERT INTO cats (name, color_id) values (?1, ?2)",
                &[&cat.to_string(), &last_id],
            )?;
        }
    }
    let mut stmt = conn.prepare(
        "SELECT c.name, cc.name from cats c
         INNER JOIN cat_colors cc
         ON cc.id = c.color_id;",
    )?;

    let cats = stmt.query_map(NO_PARAMS, |row| {
        Ok(Cat {
            name: row.get(0)?,
            color: row.get(1)?,
        })
    })?;

    for cat in cats {
        println!("Found cat {:?}", cat);
    }

    Ok(())
}
```

[`Connection::open`]: https://docs.rs/rusqlite/*/rusqlite/struct.Connection.html#method.open
[`prepare`]: https://docs.rs/rusqlite/*/rusqlite/struct.Connection.html#method.prepare
[`statement`]: https://docs.rs/rusqlite/*/rusqlite/struct.Statement.html
[`query_map`]: https://docs.rs/rusqlite/*/rusqlite/struct.Statement.html#method.query_map
[`execute`]: https://docs.rs/rusqlite/*/rusqlite/struct.Connection.html#method.execute
[`last_insert_rowid`]: https://docs.rs/rusqlite/*/rusqlite/struct.Connection.html#method.last_insert_rowid
