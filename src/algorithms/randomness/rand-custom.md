## 生成自定义类型随机值

<!--
> [algorithms/randomness/rand-custom.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/algorithms/randomness/rand-custom.md)
> <br />
> commit - 203b1085212a7b857d9a29bdc6a763515e77e0f9 - 2020.06.08
-->

[![rand-badge]][rand] [![cat-science-badge]][cat-science]

随机生成一个元组 `(i32, bool, f64)` 和用户定义类型为 `Point` 的变量。为  [`Standard`]  实现 [`Distribution`] trait，以允许随机生成。

```rust,edition2018
use rand::Rng;
use rand::distributions::{Distribution, Standard};

#[derive(Debug)]
struct Point {
    x: i32,
    y: i32,
}

impl Distribution<Point> for Standard {
    fn sample<R: Rng + ?Sized>(&self, rng: &mut R) -> Point {
        let (rand_x, rand_y) = rng.gen();
        Point {
            x: rand_x,
            y: rand_y,
        }
    }
}

fn main() {
    let mut rng = rand::thread_rng();
    let rand_tuple = rng.gen::<(i32, bool, f64)>();
    let rand_point: Point = rng.gen();
    println!("Random tuple: {:?}", rand_tuple);
    println!("Random Point: {:?}", rand_point);
}
```

[`Distribution`]: https://docs.rs/rand/*/rand/distributions/trait.Distribution.html
[`Standard`]: https://docs.rs/rand/*/rand/distributions/struct.Standard.html
