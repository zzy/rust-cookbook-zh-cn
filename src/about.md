# 关于本书

<!--
> [about.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/about.md)
> <br />
> commit - b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

## 目录

- [指南目标读者](#指南目标读者)
- [如何阅读指南](#如何阅读指南)
- [如何使用实例](#如何使用实例)
- [关于错误处理](#关于错误处理)
- [关于 crate 的说明](#关于-crate-的说明)

## 指南目标读者

本指南适用于 Rust 程序员新手，以便于他们可以快速了解 Rust crate 生态系统的功能。同时，本指南也适用于经验丰富的 Rust 程序员，他们可以在本指南中找到如何完成常见任务的简单提示。

## 如何阅读指南

指南[索引][index]包含完整的实例列表，组织为数个章节：基础、编码、并发等。这些章节基本是按照顺序排列的；后面的章节更深入一些，并且有些实例是在前面章节的概念之上构建。

在[索引][index]中，每个章节都包含实例列表。实例名称是要完成任务的简单描述，比如“在一个范围内生成随机数”；每个实例都有标记指示所使用的 _crates_，比如 [![rand-badge]][rand]；以及 crate 在 [crates.io] 上的分类，比如 [![cat-science-badge]][cat-science]。

Rust 程序员新手应该按照由第一章节直至最后章节的顺序来阅读，这种方式易于理解。同时，这样也可以对 crate 生态系统有一个全面的了解。点击索引中的章节标题，或者在侧边栏中导航到指南的章节页面。

如果你只是在简单地为一个任务的寻找解决方案，那么指南就较难以导航。找到特定实例的最简单的方法是详细查看索引，寻找感兴趣的 crate 及其类别，然后点击实例的名称来阅读它。指南的导航和浏览还在改进，以后或许会有所改善。

## 如何使用实例

指南的设计是为了让你能够即时访问可工作的代码，以及对其正在做什么有一个完整阐述，并指导你了解如何更进一步的信息。

指南中的所有实例都是完整的、可独立运行的程序，因此你可以直接复制它们到自己的项目中进行试验。为此，请按照以下说明进行操作。

考虑这个实例：“在一个范围内，生成随机数”：

[![rand-badge]][rand] [![cat-science-badge]][cat-science]

```rust,edition2018
use rand::Rng;

fn main() {
    let mut rng = rand::thread_rng();
    println!("Random f64: {}", rng.gen::<f64>());
}
```

欲在本地使用，我们可以运行以下命令来创建一个新的 cargo 项目，并切换到该目录：

```sh
cargo new my-example --bin
cd my-example
```

然后，我们还需要添加必要的 crate 到 [Cargo.toml] 中（译者注：了解更多请阅读 [Cargo 中文文档](https://books.budshome.com/cargo)，关于 Cargo.toml 详细信息也可参阅 [Cargo.toml 与 Cargo.lock](https://books.budshome.com/cargo/guide/cargo-toml-vs-cargo-lock.html) 中文文档），如上面示例代码顶部的 crate 标志所示，在本例中仅使用了 “rand” crate。为了增加 “rand” crate，我们将使用 `cargo add` 命令，该命令由 [`cargo-edit`] crate 提供，我们需要先安装它：

```sh
cargo install cargo-edit
cargo add rand
```

接下来，可以使用示例代码替换 `src/main.rs` 文件的全部内容，并通过如下命令运行：

```sh
cargo run
```

> 亲，成功执行了吧？你已经是一个 Rustacean（Rust 开发者）了！——此处应该有掌声 :-)

上面示例代码顶部的 crate 标志，链接到了 crate 在站点 [docs.rs] 上的完整文档，通常你在决定使用那个 crate 组件后，应该阅读一下它的文档。

## 关于错误处理

正确处理时，Rust 中的错误处理是健壮的，但在现今的 Rust 中，它需要大量的模板文件。正因为如此，你会发现 Rust 示例中经常充满了 `unwrap` 调用，而不是正确的错误处理。

由于本指南旨在提供原样重用的实例，并鼓励最佳实践。因此当涉及到 `Result` 类型时，它们可以正确地设置错误处理。

我们使用的基本模式是有一个 `fn main() -> Result` 函数签名。

错误处理的结构，通常如下：

```rust,edition2018
use error_chain::error_chain;
use std::net::IpAddr;
use std::str;

error_chain! {
    foreign_links {
        Utf8(std::str::Utf8Error);
        AddrParse(std::net::AddrParseError);
    }
}

fn main() -> Result<()> {
    let bytes = b"2001:db8::1";

    // Bytes 格式化为 string
    let s = str::from_utf8(bytes)?;

    // String 解析为 IP address
    let addr: IpAddr = s.parse()?;

    println!("{:?}", addr);
    Ok(())
}

```

使用 `error_chain!` 宏自定义 `Error` 和 `Result` 类型，以及来自两种标准库的错误类型的自动转换。自动转换使得 `?` 操作符正常运作。

在默认情况下——基于可读性目的——错误处理模板是隐藏的，如下代码所示。要阅读完整代码，请单击位于代码段右上角的“展开”（<i class="fa fa-expand"></i>）按钮。

```rust,edition2018
# use error_chain::error_chain;

use url::{Url, Position};
#
# error_chain! {
#     foreign_links {
#         UrlParse(url::ParseError);
#     }
# }

fn main() -> Result<()> {
    let parsed = Url::parse("https://httpbin.org/cookies/set?k2=v2&k1=v1")?;
    let cleaned: &str = &parsed[..Position::AfterPath];
    println!("cleaned: {}", cleaned);
    Ok(())
}
```

要了解更多关于 Rust 错误处理的背景知识，请阅读 [Rust 程序设计语言（2018）中的错误处理章节][error-docs]，以及这篇[博客文章][error-blog]。

## 关于 crate 的说明

本指南的最终目的是对 Rust crate 生态系统的广泛覆盖，但目前还处于引导和演示过程中，因此覆盖范围有限。我们希望从小范围开始，然后慢慢拓展，这将有助于指南更快地成为高质量的资源，并使其在增长过程中保持一致的质量水平。

目前，本指南聚焦在标准库、以及“核心”或“基础”方面的 crate ——这些 crate 构成了最常见的编程任务，而生态系统的其它部分则是以此为基础构建的。

本指南与 [Rust Libz Blitz] 密切相关，后者是一个用于识别并提高 crate 质量的项目，因此它在很大程度上推迟了 crate 的选择。任何已经走完评估过程的 crate，都在本指南的撰写范围内，而且等待评估的 crate 也是如此。

{{#include links.md}}

[index]: intro.md
[error-docs]: https://books.budshome.com/rust-lang/ch09-00-error-handling.html
[error-blog]: https://brson.github.io/2016/11/30/starting-with-error-chain
[error-chain]: https://docs.rs/error-chain/
[Rust Libz Blitz]: https://internals.rust-lang.org/t/rust-libz-blitz/5184
[crates.io]: https://crates.io
[docs.rs]: https://docs.rs
[Cargo.toml]: http://doc.crates.io/manifest.html
[`cargo-edit`]: https://github.com/killercup/cargo-edit
