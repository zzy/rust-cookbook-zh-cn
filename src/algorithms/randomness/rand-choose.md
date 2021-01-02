## 从一组用户定义字符创建随机密码

<!--
> [algorithms/randomness/rand-choose.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/algorithms/randomness/rand-choose.md)
> <br />
> commit - 8d0d2e3fb1f26e4e79a914ea332096a0ba2ba2da - 2021.01.02
-->

[![rand-badge]][rand] [![cat-os-badge]][cat-os]

使用用户自定义的字节字符串，使用 [`gen_range`] 函数，随机生成一个给定长度的 ASCII 字符串。

```rust,edition2018
fn main() {
    use rand::Rng;
    const CHARSET: &[u8] = b"ABCDEFGHIJKLMNOPQRSTUVWXYZ\
                            abcdefghijklmnopqrstuvwxyz\
                            0123456789)(*&^%$#@!~";
    const PASSWORD_LEN: usize = 30;
    let mut rng = rand::thread_rng();

    let password: String = (0..PASSWORD_LEN)
        .map(|_| {
            let idx = rng.gen_range(0..CHARSET.len());
            CHARSET[idx] as char
        })
        .collect();

    println!("{:?}", password);
}
```

[`gen_range`]: https://docs.rs/rand/*/rand/trait.Rng.html#method.gen_range
