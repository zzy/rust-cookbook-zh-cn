## 地球上两点之间的距离

<!--
> [science/mathematics/trigonometry/latitude-longitude.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/science/mathematics/trigonometry/latitude-longitude.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std]

默认情况下，Rust 提供了数学上的[浮点数方法][float methods]，例如：三角函数、平方根、弧度和度数之间的转换函数等。

下面的实例使用[半正矢公式][Haversine formula]计算地球上两点之间的距离（以公里为单位）。两个点用一对经纬度表示，然后，[`to_radians`] 将它们转换为弧度。[`sin`]、[`cos`]、[`powi`] 以及 [`sqrt`] 计算中心角。最终，可以计算出距离。

```rust,edition2018
fn main() {
    let earth_radius_kilometer = 6371.0_f64;
    let (paris_latitude_degrees, paris_longitude_degrees) = (48.85341_f64, -2.34880_f64);
    let (london_latitude_degrees, london_longitude_degrees) = (51.50853_f64, -0.12574_f64);

    let paris_latitude = paris_latitude_degrees.to_radians();
    let london_latitude = london_latitude_degrees.to_radians();

    let delta_latitude = (paris_latitude_degrees - london_latitude_degrees).to_radians();
    let delta_longitude = (paris_longitude_degrees - london_longitude_degrees).to_radians();

    let central_angle_inner = (delta_latitude / 2.0).sin().powi(2)
        + paris_latitude.cos() * london_latitude.cos() * (delta_longitude / 2.0).sin().powi(2);
    let central_angle = 2.0 * central_angle_inner.sqrt().asin();

    let distance = earth_radius_kilometer * central_angle;

    println!(
        "Distance between Paris and London on the surface of Earth is {:.1} kilometers",
        distance
    );
}
```

[float methods]: https://doc.rust-lang.org/std/primitive.f64.html#methods
[`to_radians`]: https://doc.rust-lang.org/std/primitive.f64.html#method.to_radians
[`sin`]: https://doc.rust-lang.org/std/primitive.f64.html#method.sin
[`cos`]: https://doc.rust-lang.org/std/primitive.f64.html#method.cos
[`powi`]: https://doc.rust-lang.org/std/primitive.f64.html#method.powi
[`sqrt`]: https://doc.rust-lang.org/std/primitive.f64.html#method.sqrt
[Haversine formula]: https://en.wikipedia.org/wiki/Haversine_formula
