## ANSI 终端

<!--
> [cli/ansi_terminal/ansi_term-basic.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/cli/ansi_terminal/ansi_term-basic.md)
> <br />
> commit - ffaa6acec85e1837558384cc7ac191715931f8c9 - 2018.06.14
-->

[![ansi_term-badge]][ansi_term] [![cat-command-line-badge]][cat-command-line]

此程序描述了 [`ansi_term`] crate 的使用方法，以及它如何用于控制 ANSI 终端上的颜色和格式，如蓝色粗体文本或黄色下划线文本。

[`ansi_term`] 中有两种主要的数据结构：[`ANSIString`] 和 [`Style`]。[`Style`] 包含样式信息：颜色，是否粗体文本，或者是否闪烁，或者其它样式。也有 Colour 变体，代表简单的前景色样式。[`ANSIString`] 是与 [`Style`] 配对的字符串。

**注意**：英式英语中使用 *Colour* 而不是 *Color*，不要混淆。

### 打印彩色文本到终端

```rust,edition2018
use ansi_term::Colour;

fn main() {
    println!("This is {} in color, {} in color and {} in color",
             Colour::Red.paint("red"),
             Colour::Blue.paint("blue"),
             Colour::Green.paint("green"));
}
```

### 终端中的粗体文本

对于比简单的前景色变化更复杂的事情，代码需要构造 `Style` 结构体。[`Style::new()`] 创建结构体，并链接属性。

```rust,edition2018
use ansi_term::Style;

fn main() {
    println!("{} and this is not",
             Style::new().bold().paint("This is Bold"));
}
```
### 终端中的粗体和彩色文本

`Colour` 模块实现了许多类似 `Style` 的函数，并且可以链接方法。

```rust,edition2018
use ansi_term::Colour;
use ansi_term::Style;

fn main(){
    println!("{}, {} and {}",
             Colour::Yellow.paint("This is colored"),
             Style::new().bold().paint("this is bold"),
             Colour::Yellow.bold().paint("this is bold and colored"));
}
```

[documentation]: https://docs.rs/ansi_term/
[`ansi_term`]: https://crates.io/crates/ansi_term
[`ANSIString`]: https://docs.rs/ansi_term/*/ansi_term/type.ANSIString.html
[`Style`]: https://docs.rs/ansi_term/*/ansi_term/struct.Style.html
[`Style::new()`]: https://docs.rs/ansi_term/0.11.0/ansi_term/struct.Style.html#method.new
