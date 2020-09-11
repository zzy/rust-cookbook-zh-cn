## Map-reduce 并行计算

<!--
> [concurrency/parallel/rayon-map-reduce.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/concurrency/parallel/rayon-map-reduce.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![rayon-badge]][rayon] [![cat-concurrency-badge]][cat-concurrency]

此实例使用 [`rayon::filter`]、[`rayon::map`]，以及 [`rayon::reduce`] 计算 `Person` 对象中年龄超过 30 岁的那些人的平均年龄。

[`rayon::filter`] 过滤集合中满足给定断言的元素。[`rayon::map`] 对每个元素执行一次计算，创建一个新的迭代；然后，基于前一次的 reduce 计算结果和当前元素一起，[`rayon::reduce`] 执行新的计算。也可以查看 [`rayon::sum`]，它与本实例中的 reduce 计算具有相同的结果。

```rust,edition2018
use rayon::prelude::*;

struct Person {
    age: u32,
}

fn main() {
    let v: Vec<Person> = vec![
        Person { age: 23 },
        Person { age: 19 },
        Person { age: 42 },
        Person { age: 17 },
        Person { age: 17 },
        Person { age: 31 },
        Person { age: 30 },
    ];

    let num_over_30 = v.par_iter().filter(|&x| x.age > 30).count() as f32;
    let sum_over_30 = v.par_iter()
        .map(|x| x.age)
        .filter(|&x| x > 30)
        .reduce(|| 0, |x, y| x + y);

    let alt_sum_30: u32 = v.par_iter()
        .map(|x| x.age)
        .filter(|&x| x > 30)
        .sum();

    let avg_over_30 = sum_over_30 as f32 / num_over_30;
    let alt_avg_over_30 = alt_sum_30 as f32/ num_over_30;

    assert!((avg_over_30 - alt_avg_over_30).abs() < std::f32::EPSILON);
    println!("The average age of people older than 30 is {}", avg_over_30);
}
```

[`rayon::filter`]: https://docs.rs/rayon/*/rayon/iter/trait.ParallelIterator.html#method.filter
[`rayon::map`]: https://docs.rs/rayon/*/rayon/iter/trait.ParallelIterator.html#method.map
[`rayon::reduce`]: https://docs.rs/rayon/*/rayon/iter/trait.ParallelIterator.html#method.reduce
[`rayon::sum`]: https://docs.rs/rayon/*/rayon/iter/trait.ParallelIterator.html#method.sum
