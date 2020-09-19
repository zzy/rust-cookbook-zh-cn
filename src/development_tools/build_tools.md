# 构建工具

<!--
> [development_tools/build_tools.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/build_tools.md)
> <br />
> commit 97dabe59ae705bf6a2aaebbcd1d189ec2a83f98b - 2018.07.11
-->

本节介绍在编译 crate 源代码之前运行的“构建时”工具或代码。按照惯例，构建时代码存放在 **build.rs** 文件，通常称为“构建脚本”。常见的用例包括：Rust 代码生成、绑定的 C/C++/asm 代码的编译。要获取更多信息，请阅读 Cargo（[中文文档](https://cargo.budshome.com)） 的[构建脚本文档][build-script-docs]。

{{#include build_tools/cc-bundled-static.md}}

{{#include build_tools/cc-bundled-cpp.md}}

{{#include build_tools/cc-defines.md}}

{{#include ../links.md}}

[build-script-docs]: https://cargo.budshome.com/reference/build-scripts.html
