# 编码

<!--
> [encoding.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/encoding.md)
> <br />
> commit 7a06c79008f3998fc03d23c56aee885ddcf366ac - 2020.05.05
-->

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [百分比编码（URL 编码）字符串][ex-percent-encode] | [![percent-encoding-badge]][url] | [![cat-encoding-badge]][cat-encoding] |
| [将字符串编码为 application/x-www-form-urlencoded][ex-urlencoded] | [![url-badge]][url] | [![cat-encoding-badge]][cat-encoding] |
| [编码和解码十六进制][ex-hex-encode-decode] | [![data-encoding-badge]][data-encoding] | [![cat-encoding-badge]][cat-encoding] |
| [编码和解码 base64][ex-base64] | [![base64-badge]][base64] | [![cat-encoding-badge]][cat-encoding] |
| [读取 CSV 记录][ex-csv-read] | [![csv-badge]][csv] | [![cat-encoding-badge]][cat-encoding] |
| [读取有不同分隔符的 CSV 记录][ex-csv-delimiter] | [![csv-badge]][csv] | [![cat-encoding-badge]][cat-encoding] |
| [筛选匹配断言的 CSV 记录][ex-csv-filter] | [![csv-badge]][csv] | [![cat-encoding-badge]][cat-encoding] |
| [用 Serde 处理无效的 CSV 数据][ex-invalid-csv] | [![csv-badge]][csv] [![serde-badge]][serde] | [![cat-encoding-badge]][cat-encoding] |
| [将记录序列化为 CSV][ex-serialize-csv] | [![csv-badge]][csv] | [![cat-encoding-badge]][cat-encoding] |
| [用 Serde 将记录序列化为 CSV][ex-csv-serde] | [![csv-badge]][csv] [![serde-badge]][serde] | [![cat-encoding-badge]][cat-encoding] |
| [转换 CSV 文件的列][ex-csv-transform-column] | [![csv-badge]][csv] [![serde-badge]][serde] | [![cat-encoding-badge]][cat-encoding] |
| [将非结构化 JSON 序列化和反序列化][ex-json-value] | [![serde-json-badge]][serde-json] | [![cat-encoding-badge]][cat-encoding] |
| [反序列化 TOML 配置文件][ex-toml-config] | [![toml-badge]][toml] | [![cat-encoding-badge]][cat-encoding] |
| [以小端字节顺序读写整数][ex-byteorder-le] | [![byteorder-badge]][byteorder] | [![cat-encoding-badge]][cat-encoding] |

[ex-percent-encode]: encoding/strings.md#百分比编码url-编码字符串
[ex-urlencoded]: encoding/strings.md#将字符串编码为-applicationx-www-form-urlencoded
[ex-hex-encode-decode]: encoding/strings.md#编码和解码十六进制
[ex-base64]: encoding/strings.md#编码和解码-base64
[ex-csv-read]: encoding/csv.md#读取-csv-记录
[ex-csv-delimiter]: encoding/csv.md#读取有不同分隔符的-csv-记录
[ex-csv-filter]: encoding/csv.md#筛选匹配断言的-csv-记录
[ex-invalid-csv]: encoding/csv.md#用-serde-处理无效的-csv-数据
[ex-serialize-csv]: encoding/csv.md#将记录序列化为-csv
[ex-csv-serde]: encoding/csv.md#用-serde-将记录序列化为-csv
[ex-csv-transform-column]: encoding/csv.md#转换-csv-文件的列
[ex-json-value]: encoding/complex.md#将非结构化-json-序列化和反序列化
[ex-toml-config]: encoding/complex.md#反序列化-toml-配置文件
[ex-byteorder-le]: encoding/complex.md#以小端字节顺序读写整数


{{#include links.md}}
