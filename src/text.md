# 文本处理

<!--
> [os.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/text.md)
> <br />
> commit f1ad9ad44c355d12b7759dca2f700f254ca86114 - 2019.04.13
-->

## 正则表达式

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [验证并提取电子邮件登录信息][ex-verify-extract-email] | [![regex-badge]][regex] [![lazy_static-badge]][lazy_static] | [![cat-text-processing-badge]][cat-text-processing] |
| [从文本提取标签元素唯一的列表][ex-extract-hashtags] | [![regex-badge]][regex] [![lazy_static-badge]][lazy_static] | [![cat-text-processing-badge]][cat-text-processing] |
| [从文本提取电话号码][ex-phone] | [![regex-badge]][regex] | [![cat-text-processing-badge]][cat-text-processing] |
| [通过匹配多个正则表达式来筛选日志文件][ex-regex-filter-log] | [![regex-badge]][regex] | [![cat-text-processing-badge]][cat-text-processing]
| [文本模式提换][ex-regex-replace-named] | [![regex-badge]][regex] [![lazy_static-badge]][lazy_static] | [![cat-text-processing-badge]][cat-text-processing] |

## 字符串解析

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [收集 Unicode 字符][ex-unicode-graphemes] | [![unicode-segmentation-badge]][unicode-segmentation] | [![cat-encoding-badge]][cat-text-processing] |
| [自定义`结构体`并实现 `FromStr` trait][string_parsing-from_str] | [![std-badge]][std] | [![cat-text-processing-badge]][cat-text-processing] |

[ex-verify-extract-email]: text/regex.md#验证并提取电子邮件登录信息
[ex-extract-hashtags]: text/regex.md#从文本提取标签元素唯一的列表
[ex-phone]: text/regex.md#从文本提取电话号码
[ex-regex-filter-log]: text/regex.md#通过匹配多个正则表达式来筛选日志文件
[ex-regex-replace-named]: text/regex.md#文本模式提换

[ex-unicode-graphemes]: text/string_parsing.md#收集-unicode-字符
[string_parsing-from_str]: text/string_parsing.md#自定义结构体并实现-fromstr-trait

{{#include links.md}}
