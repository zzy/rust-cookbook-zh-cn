## 解析命令行参数

<!--
> [cli/arguments/clap-basic.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/cli/arguments/clap-basic.md)
> <br />
> commit - b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![clap-badge]][clap] [![cat-command-line-badge]][cat-command-line]

此应用程序使用 `clap` 构建器样式描述其命令行界面的结构。[文档][documentation]还提供了另外两种可用的方法去实例化应用程序。

在构建器样式中，`with_name` 函数是 `value_of` 方法将用于检索传递值的唯一标识符。`short` 和 `long` 选项控制用户将要键入的标志；short 标志看起来像 `-f`，long 标志看起来像 `--file`。

```rust,edition2018
use clap::{Arg, App};

fn main() {
    let matches = App::new("My Test Program")
        .version("0.1.0")
        .author("Hackerman Jones <hckrmnjones@hack.gov>")
        .about("Teaches argument parsing")
        .arg(Arg::with_name("file")
                 .short("f")
                 .long("file")
                 .takes_value(true)
                 .help("A cool file"))
        .arg(Arg::with_name("num")
                 .short("n")
                 .long("number")
                 .takes_value(true)
                 .help("Five less than your favorite number"))
        .get_matches();

    let myfile = matches.value_of("file").unwrap_or("input.txt");
    println!("The file passed is: {}", myfile);

    let num_str = matches.value_of("num");
    match num_str {
        None => println!("No idea what your favorite number is."),
        Some(s) => {
            match s.parse::<i32>() {
                Ok(n) => println!("Your favorite number must be {}.", n + 5),
                Err(_) => println!("That's not a number! {}", s),
            }
        }
    }
}
```

使用方法信息由 `clap` 生成。示例应用程序的用法如下所示。

```
My Test Program 0.1.0
Hackerman Jones <hckrmnjones@hack.gov>
Teaches argument parsing

USAGE:
    testing [OPTIONS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
    -f, --file <file>     A cool file
    -n, --number <num>    Five less than your favorite number
```

我们可以通过运行如下命令来测试应用程序。

```
$ cargo run -- -f myfile.txt -n 251
```

输出为：

```
The file passed is: myfile.txt
Your favorite number must be 256.
```

[documentation]: https://docs.rs/clap/
