## 从一组字母数字字符创建随机密码

<!--
> [algorithms/randomness/rand-passwd.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/algorithms/randomness/rand-passwd.md)
> <br />
> commit - 8d0d2e3fb1f26e4e79a914ea332096a0ba2ba2da - 2021.01.02
-->

[![rand-badge]][rand] [![cat-os-badge]][cat-os]

随机生成一个给定长度的 ASCII 字符串，范围为 `A-Z，a-z，0-9`，使用[字母数字][`Alphanumeric`]样本。

```rust,edition2018
use rand::{thread_rng, Rng};
use rand::distributions::Alphanumeric;

fn main() {
    let rand_string: String = thread_rng()
        .sample_iter(&Alphanumeric)
        .take(30)
        .map(char::from)
        .collect();

    println!("{}", rand_string);
}
```

[`Alphanumeric`]: https://docs.rs/rand/*/rand/distributions/struct.Alphanumeric.html
