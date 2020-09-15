## 事务处理

<!--
> [database/sqlite/transactions.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/database/sqlite/transactions.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![rusqlite-badge]][rusqlite] [![cat-database-badge]][cat-database]

[`Connection::open`] 将打开来自前述实例的数据库 `cats.db`。

使用 [`Connection::transaction`] 开始事务，除非使用 [`Transaction::commit`] 显式提交，否则事务将回滚。

在下面的实例中，颜色表对颜色名称具有唯一性约束。当尝试插入重复的颜色时，事务会回滚。

```rust,edition2018,no_run
use rusqlite::{Connection, Result, NO_PARAMS};

fn main() -> Result<()> {
    let mut conn = Connection::open("cats.db")?;

    successful_tx(&mut conn)?;

    let res = rolled_back_tx(&mut conn);
    assert!(res.is_err());

    Ok(())
}

fn successful_tx(conn: &mut Connection) -> Result<()> {
    let tx = conn.transaction()?;

    tx.execute("delete from cat_colors", NO_PARAMS)?;
    tx.execute("insert into cat_colors (name) values (?1)", &[&"lavender"])?;
    tx.execute("insert into cat_colors (name) values (?1)", &[&"blue"])?;

    tx.commit()
}

fn rolled_back_tx(conn: &mut Connection) -> Result<()> {
    let tx = conn.transaction()?;

    tx.execute("delete from cat_colors", NO_PARAMS)?;
    tx.execute("insert into cat_colors (name) values (?1)", &[&"lavender"])?;
    tx.execute("insert into cat_colors (name) values (?1)", &[&"blue"])?;
    tx.execute("insert into cat_colors (name) values (?1)", &[&"lavender"])?;

    tx.commit()
}
```

[`Connection::transaction`]: https://docs.rs/rusqlite/*/rusqlite/struct.Connection.html#method.transaction
[`Transaction::commit`]: https://docs.rs/rusqlite/*/rusqlite/struct.Transaction.html#method.commit
