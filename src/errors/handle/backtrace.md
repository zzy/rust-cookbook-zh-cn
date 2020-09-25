## 获取复杂错误场景的回溯

<!--
> [errors/handle/backtrace.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/errors/handle/backtrace.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![error-chain-badge]][error-chain] [![cat-rust-patterns-badge]][cat-rust-patterns]

本实例展示了如何处理一个复杂的错误场景，并且打印出错误回溯。依赖于 [`chain_err`]，通过附加新的错误来扩展错误信息。从而可以展开错误堆栈，这样提供了更好的上下文来理解错误的产生原因。

下述代码尝试将值 `256` 反序列化为 `u8`。首先 Serde 产生错误，然后是 csv，最后是用户代码。

```rust,edition2018
use error_chain::error_chain;
# use serde::Deserialize;
#
# use std::fmt;
#
# error_chain! {
#     foreign_links {
#         Reader(csv::Error);
#     }
# }

#[derive(Debug, Deserialize)]
struct Rgb {
    red: u8,
    blue: u8,
    green: u8,
}

impl Rgb {
    fn from_reader(csv_data: &[u8]) -> Result<Rgb> {
        let color: Rgb = csv::Reader::from_reader(csv_data)
            .deserialize()
            .nth(0)
            .ok_or("Cannot deserialize the first CSV record")?
            .chain_err(|| "Cannot deserialize RGB color")?;

        Ok(color)
    }
}

# impl fmt::UpperHex for Rgb {
#     fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
#         let hexa = u32::from(self.red) << 16 | u32::from(self.blue) << 8 | u32::from(self.green);
#         write!(f, "{:X}", hexa)
#     }
# }
#
fn run() -> Result<()> {
    let csv = "red,blue,green
102,256,204";

    let rgb = Rgb::from_reader(csv.as_bytes()).chain_err(|| "Cannot read CSV data")?;
    println!("{:?} to hexadecimal #{:X}", rgb, rgb);

    Ok(())
}

fn main() {
    if let Err(ref errors) = run() {
        eprintln!("Error level - description");
        errors
            .iter()
            .enumerate()
            .for_each(|(index, error)| eprintln!("└> {} - {}", index, error));

        if let Some(backtrace) = errors.backtrace() {
            eprintln!("{:?}", backtrace);
        }
#
#         // In a real use case, errors should handled. For example:
#         // ::std::process::exit(1);
    }
}
```

错误回溯信息如下：

```text
Error level - description
└> 0 - Cannot read CSV data
└> 1 - Cannot deserialize RGB color
└> 2 - CSV deserialize error: record 1 (line: 2, byte: 15): field 1: number too large to fit in target type
└> 3 - field 1: number too large to fit in target type
```

可以通过附加命令参数 `RUST_BACKTRACE=1` 运行实例，以显示与此错误相关的详细[回溯][`backtrace`]。

[`backtrace`]: https://docs.rs/error-chain/*/error_chain/trait.ChainedError.html#tymethod.backtrace
[`chain_err`]: https://docs.rs/error-chain/*/error_chain/index.html#chaining-errors
