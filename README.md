# Rust Cookbook 中文版 &emsp; [![Build Status travis]][travis]

[Build Status travis]: https://api.travis-ci.com/zzy/rust-cookbook-zh-cn.svg?branch=master
[travis]: https://travis-ci.com/zzy/rust-cookbook-zh-cn

《Rust Cookbook 中文版》是 Rust 程序设计语言（[Rust 2018 简体中文版文档](https://rust-lang.budshome.com)）的简要实例示例集合：展示了在 Rust 生态系统中，使用各类 crate 来完成常见编程任务的良好实践。

书中的实例都是完整的，并且经过测试，保证能正常工作，可以直接复制到你新建的 Cargo（[中文文档](https://cargo.budshome.com)）项目中进行使用。

> 注1：《Rust Cookbook 中文版》翻译自 rust-lang-nursery 团队撰写的 _"A Rust Cookbook"_，感谢 rust-lang-nursery 团队的无私奉献！

> 注2：《Rust Cookbook 中文版》计划为两个阶段：<br>
> 第一阶段：经仔细斟酌，形成专业、通俗、容易理解的 Rust 生态实践指南中文版本；<br>
> 第二阶段：对书中代码进行详细讲解，在实际应用场景中对 Rust 生态 crate 进行分析、比较，以及拓展。<br>
> 目前，第一阶段已经完成，并且和 rust-lang-nursery 团队项目同步更新。<br>
> 第二阶段正在进行中，但可能需要另开一个开源项目，欢迎参与。<br>
> 若需要，欢迎联系：linshi@budshome.com。

## 在线阅读

在线阅读地址：[**《Rust Cookbook 中文版》** - http://rust-cookbook.budshome.com](http://rust-cookbook.budshome.com)。

欢迎您提出宝贵意见：linshi@budshome.com，budshome（个人微信），晨曦中（微信公众号）。

## 离线阅读

如果你喜欢本地阅读方式，可以使用 mdBook（[中文文档](https://mdbook.budshome.com)） 进行书籍构建：

```bash
$ git clone https://github.com/zzy/rust-cookbook-zh-cn
$ cd rust-cookbook-zh-cn
$ cargo install mdbook # 指定版本使用参数：--vers "0.3.5"
$ mdbook serve --open # 或者 mdbook build
```

也可以直接用你喜欢的浏览器从 `book` 子目录打开 `index.html` 文件。

```bash
$ xdg-open ./book/index.html # linux
$ start .\book\index.html    # windows
$ open ./book/index.html     # mac
```

## 测试

如果欲运行构建本书的测试组件，请执行：

> 测试组件需要安装一些 crate，中国大陆推荐[更换默认的 Cargo 源为国内镜像源](https://cargo.budshome.com/reference/source-replacement.html)。

```bash
$ cargo test
```

## 贡献

《Rust Cookbook 中文版》的目的是让 Rust 程序员新手能够更容易地参与到 Rust 社区中，因此它需要——并欢迎——你做出自己力所能及的贡献。

### 构建和测试

首先，从 git 克隆《Rust Cookbook 中文版》并进入目录：

```
git clone https://github.com/zzy/rust-cookbook-zh-cn.git
cd rust-cookbook-zh-cn
```

《Rust Cookbook 中文版》使用 `mdBook`（[中文文档](https://mdbook.budshome.com)）构建，所以需要通过 `Cargo`（[中文文档](https://cargo.budshome.com)）安装它：

```
cargo install --version 0.3.5 mdbook
```

若要在本地生成和阅读《Rust Cookbook 中文版》，请运行：

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
