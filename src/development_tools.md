# 开发工具

<!--
> [development_tools.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools.md)
> <br />
> commit 97dabe59ae705bf6a2aaebbcd1d189ec2a83f98b - 2018.07.11
-->

{{#include development_tools/debugging.md}}

## 版本控制

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [解析并递增版本字符串][ex-semver-increment] | [![semver-badge]][semver] | [![cat-config-badge]][cat-config] |
| [解析复杂的版本字符串][ex-semver-complex] | [![semver-badge]][semver] | [![cat-config-badge]][cat-config] |
| [检查给定版本是否为预发布版本][ex-semver-prerelease] | [![semver-badge]][semver] | [![cat-config-badge]][cat-config] |
| [查询适配给定范围的最新版本][ex-semver-latest] | [![semver-badge]][semver] | [![cat-config-badge]][cat-config] |
| [检查外部命令的版本兼容性][ex-semver-command] | [![semver-badge]][semver] | [![cat-text-processing-badge]][cat-text-processing] [![cat-os-badge]][cat-os]

## 构建时

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [静态编译并链接到绑定的 C 语言库][ex-cc-static-bundled] | [![cc-badge]][cc] | [![cat-development-tools-badge]][cat-development-tools] |
| [静态编译并链接到绑定的 C++ 语言库][ex-cc-static-bundled-cpp] | [![cc-badge]][cc] | [![cat-development-tools-badge]][cat-development-tools] |
| [编译 C 语言库时自定义设置][ex-cc-custom-defines] | [![cc-badge]][cc] | [![cat-development-tools-badge]][cat-development-tools] |

[ex-semver-increment]: development_tools/versioning.md#解析并递增版本字符串
[ex-semver-complex]: development_tools/versioning.md#解析复杂的版本字符串
[ex-semver-prerelease]: development_tools/versioning.md#检查给定版本是否为预发布版本
[ex-semver-latest]: development_tools/versioning.md#查询适配给定范围的最新版本
[ex-semver-command]: development_tools/versioning.md#检查外部命令的版本兼容性

[ex-cc-static-bundled]: development_tools/build_tools.md#静态编译并链接到绑定的-c-语言库
[ex-cc-static-bundled-cpp]: development_tools/build_tools.md#静态编译并链接到绑定的-c-语言库-1
[ex-cc-custom-defines]: development_tools/build_tools.md#编译-c-语言库时自定义设置

{{#include links.md}}
