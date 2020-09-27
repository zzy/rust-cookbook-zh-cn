### 计算标准偏差

<!--
> [science/mathematics/statistics/standard-deviation.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/statistics/standard-deviation.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std] [![cat-science-badge]][cat-science]

本实例计算一组测量值的标准偏差和 z 分数（z-score）。

标准偏差定义为方差的平方根（用 f32 浮点型的 [`sqrt`] 计算），其中方差是每个测量值与[`平均数`][mean]之间的平方差的[`和`][sum]除以测量次数。

z 分数（z-score）是指单个测量值偏离数据集[`平均数`][mean]的标准差数。

```rust,edition2018
fn mean(data: &[i32]) -> Option<f32> {
    let sum = data.iter().sum::<i32>() as f32;
    let count = data.len();

    match count {
        positive if positive > 0 => Some(sum / count as f32),
        _ => None,
    }
}

fn std_deviation(data: &[i32]) -> Option<f32> {
    match (mean(data), data.len()) {
        (Some(data_mean), count) if count > 0 => {
            let variance = data.iter().map(|value| {
                let diff = data_mean - (*value as f32);

                diff * diff
            }).sum::<f32>() / count as f32;

            Some(variance.sqrt())
        },
        _ => None
    }
}

fn main() {
    let data = [3, 1, 6, 1, 5, 8, 1, 8, 10, 11];

    let data_mean = mean(&data);
    println!("Mean is {:?}", data_mean);

    let data_std_deviation = std_deviation(&data);
    println!("Standard deviation is {:?}", data_std_deviation);

    let zscore = match (data_mean, data_std_deviation) {
        (Some(mean), Some(std_deviation)) => {
            let diff = data[4] as f32 - mean;

            Some(diff / std_deviation)
        },
        _ => None
    };
    println!("Z-score of data at index 4 (with value {}) is {:?}", data[4], zscore);
}
```

[sqrt]: https://doc.rust-lang.org/std/primitive.f32.html#method.sqrt
[sum]: https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.sum
[mean]: #集中趋势度量
