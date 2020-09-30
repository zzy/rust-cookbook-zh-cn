# 密码学

<!--
> [cryptography.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/cryptography.md)
> <br />
> commit 97dabe59ae705bf6a2aaebbcd1d189ec2a83f98b - 2018.07.11
-->

## 散列（哈希）

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [计算文件的 SHA-256 摘要][ex-sha-digest] | [![ring-badge]][ring] [![data-encoding-badge]][data-encoding] | [![cat-cryptography-badge]][cat-cryptography] |
| [使用 HMAC 摘要对消息进行签名和验证][ex-hmac] | [![ring-badge]][ring] | [![cat-cryptography-badge]][cat-cryptography] |

## 加密

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [使用 PBKDF2 对密码进行加密（salt）和散列（hash）运算][ex-pbkdf2] | [![ring-badge]][ring] [![data-encoding-badge]][data-encoding] | [![cat-cryptography-badge]][cat-cryptography] |

[ex-sha-digest]: cryptography/hashing.md#计算文件的-sha-256-摘要
[ex-hmac]: cryptography/hashing.md#使用-hmac-摘要对消息进行签名和验证

[ex-pbkdf2]: cryptography/encryption.md#使用-pbkdf2-对密码进行加密salt和散列hash运算

{{#include links.md}}
