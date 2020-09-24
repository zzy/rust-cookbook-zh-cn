# Rust 实践指南

<!--
> [intro.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/intro.md)
> <br />
> commit - a79932c9a624fd17977eaea5df3d5a9600d014f6 - 2018.12.05
-->

《Rust 实践指南》是 Rust 程序设计语言（[Rust 2018 简体中文版文档](https://rust-lang.budshome.com)）的简要实例示例集合：展示了在 Rust 生态系统中，使用各类 crate 来完成常见编程任务的良好实践。

了解更多关于《Rust 实践指南》一书的信息，请阅读[关于本书](about.md)，包括：如何阅读本书的提示、如何使用实例示例，以及关于注释的约定。

> 注：《Rust 实践指南》计划为两个阶段。<br>
> 第一个阶段：经仔细斟酌，形成专业、通俗、容易理解的 Rust 生态实践指南中文版本；<br>
> 第二个阶段：对书中代码进行详细讲解，在实际应用场景中对 Rust 生态 crate 进行分析、比较，以及拓展。

## 做贡献

《Rust 实践指南》的目的是让 Rust 程序员新手能够更容易地参与到 Rust 社区中，因此它需要——并欢迎——你做出自己力所能及的贡献。

### 构建和测试

首先，从 git 克隆《Rust 实践指南》并进入目录：

```
git clone https://github.com/zzy/rust-cookbook-zh-cn.git
cd rust-cookbook-zh-cn
```

《Rust 实践指南》使用 `mdBook`（[中文文档](https://mdbook.budshome.com)）构建，所以需要通过 `Cargo`（[中文文档](https://cargo.budshome.com)）安装它：

```
cargo install --version 0.3.5 mdbook
```

若要在本地生成和阅读《Rust 实践指南》，请运行：

```
mdbook serve
```

然后在浏览器中打开 `http://localhost:3000`，即可阅读本书。对源代码所做的任何更改都将自动重新生成页面，并会主动刷新浏览器，因此在编辑源码时打开浏览器窗口是很有帮助的。

书中的所有实例都是使用 [skeptic](https://github.com/brson/rust-skeptic) 测试的，它是测试任意 markdown 文档的工具，风格类似于 rustdoc。

提交前，请对整个仓库进行测试：

```
cargo test
```

祝你学习愉快，欢迎提交问题，欢迎发送 PR。

{{#include algorithms.md}}

{{#include cli.md}}

{{#include compression.md}}

{{#include concurrency.md}}

{{#include cryptography.md}}

{{#include data_structures.md}}

{{#include database.md}}

{{#include datetime.md}}

{{#include development_tools.md}}

{{#include encoding.md}}

{{#include errors.md}}

{{#include file.md}}

{{#include hardware.md}}

{{#include mem.md}}

{{#include net.md}}

{{#include os.md}}

{{#include science.md}}

{{#include text.md}}

{{#include web.md}}
