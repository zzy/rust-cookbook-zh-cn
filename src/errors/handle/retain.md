## 避免在错误转变过程中遗漏错误

<!--
> [errors/handle/retain.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/errors/handle/retain.md)
> <br />
> commit dd4efa8dcd8e611326caa01c08db8f227aa909d6 - 2020.06.07
-->

[![error-chain-badge]][error-chain] [![cat-rust-patterns-badge]][cat-rust-patterns]

[error-chain] crate 使得[匹配][matching]函数返回的不同错误类型成为可能，并且相对简洁。[`ErrorKind`] 是枚举类型，可以确定错误类型。

下文实例使用 [reqwest]::[blocking] 来查询一个随机整数生成器的 web 服务，并将服务器响应的字符串转换为整数。Rust 标准库 [reqwest] 和 web 服务都可能会产生错误，所以使用 [`foreign_links`] 定义易于辨认理解的 Rust 错误。另外，web 服务错误的 [`ErrorKind`] 变体，使用 `error_chain!` 宏的 `errors` 代码块定义。

```rust,edition2018
use error_chain::error_chain;

error_chain! {
    foreign_links {
        Io(std::io::Error);
        Reqwest(reqwest::Error);
        ParseIntError(std::num::ParseIntError);
    }
    errors { RandomResponseError(t: String) }
}

fn parse_response(response: reqwest::blocking::Response) -> Result<u32> {
  let mut body = response.text()?;
  body.pop();
  body
    .parse::<u32>()
    .chain_err(|| ErrorKind::RandomResponseError(body))
}

fn run() -> Result<()> {
  let url =
    format!("https://www.random.org/integers/?num=1&min=0&max=10&col=1&base=10&format=plain");
  let response = reqwest::blocking::get(&url)?;
  let random_value: u32 = parse_response(response)?;
  println!("a random number between 0 and 10: {}", random_value);
  Ok(())
}

fn main() {
  if let Err(error) = run() {
    match *error.kind() {
      ErrorKind::Io(_) => println!("Standard IO error: {:?}", error),
      ErrorKind::Reqwest(_) => println!("Reqwest error: {:?}", error),
      ErrorKind::ParseIntError(_) => println!("Standard parse int error: {:?}", error),
      ErrorKind::RandomResponseError(_) => println!("User defined error: {:?}", error),
      _ => println!("Other error: {:?}", error),
    }
  }
}
```

[`ErrorKind`]: https://docs.rs/error-chain/*/error_chain/example_generated/enum.ErrorKind.html
[`foreign_links`]: https://docs.rs/error-chain/*/error_chain/#foreign-links
[blocking]: https://docs.rs/reqwest/*/reqwest/blocking/index.html
[Matching]:https://docs.rs/error-chain/*/error_chain/#matching-errors
