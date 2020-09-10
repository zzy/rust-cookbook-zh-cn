## 将绘制分形的线程分派到线程池

<!--
> [concurrency/thread/threadpool-fractal.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/concurrency/thread/threadpool-fractal.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![threadpool-badge]][threadpool] [![num-badge]][num] [![num_cpus-badge]][num_cpus] [![image-badge]][image] [![cat-concurrency-badge]][cat-concurrency][![cat-science-badge]][cat-science][![cat-rendering-badge]][cat-rendering]

此实例通过从[朱莉娅集][Julia set]绘制分形来生成图像，该集合具有用于分布式计算的线程池。

<a href="https://cloud.githubusercontent.com/assets/221000/26546700/9be34e80-446b-11e7-81dc-dd9871614ea1.png"><img src="https://cloud.githubusercontent.com/assets/221000/26546700/9be34e80-446b-11e7-81dc-dd9871614ea1.png" width="300" /></a>

使用 [`ImageBuffer::new`] 为指定宽度和高度的输出图像分配内存，[`Rgb::from_channels`] 信道则计算输出图像的 RGB 像素值。使用 [`ThreadPool`] 创建线程池，线程池中的线程数量和使用 [`num_cpus::get`] 获取的系统内核数相等。[`ThreadPool::execute`] 将每个像素作为单独的作业接收。

[`mpsc::channel`] 信道接收作业，[`Receiver::recv`] 接收器则检索作业。[`ImageBuffer::put_pixel`] 处理数据，设置像素颜色。最后，[`ImageBuffer::save`] 将图像存储为 `output.png`。

```rust,edition2018,no_run
# use error_chain::error_chain;
use std::sync::mpsc::{channel, RecvError};
use threadpool::ThreadPool;
use num::complex::Complex;
use image::{ImageBuffer, Pixel, Rgb};
#
# error_chain! {
#     foreign_links {
#         MpscRecv(RecvError);
#         Io(std::io::Error);
#     }
# }
#
# // 将强度值转换为 RGB 值的函数
# // 基于 http://www.efg2.com/Lab/ScienceAndEngineering/Spectra.htm
# fn wavelength_to_rgb(wavelength: u32) -> Rgb<u8> {
#     let wave = wavelength as f32;
#
#     let (r, g, b) = match wavelength {
#         380..=439 => ((440. - wave) / (440. - 380.), 0.0, 1.0),
#         440..=489 => (0.0, (wave - 440.) / (490. - 440.), 1.0),
#         490..=509 => (0.0, 1.0, (510. - wave) / (510. - 490.)),
#         510..=579 => ((wave - 510.) / (580. - 510.), 1.0, 0.0),
#         580..=644 => (1.0, (645. - wave) / (645. - 580.), 0.0),
#         645..=780 => (1.0, 0.0, 0.0),
#         _ => (0.0, 0.0, 0.0),
#     };
#
#     let factor = match wavelength {
#         380..=419 => 0.3 + 0.7 * (wave - 380.) / (420. - 380.),
#         701..=780 => 0.3 + 0.7 * (780. - wave) / (780. - 700.),
#         _ => 1.0,
#     };
#
#     let (r, g, b) = (normalize(r, factor), normalize(g, factor), normalize(b, factor));
#     Rgb::from_channels(r, g, b, 0)
# }
#
# // 将茱莉亚集距离映射为强度值
# fn julia(c: Complex<f32>, x: u32, y: u32, width: u32, height: u32, max_iter: u32) -> u32 {
#     let width = width as f32;
#     let height = height as f32;
#
#     let mut z = Complex {
#         // scale and translate the point to image coordinates
#         re: 3.0 * (x as f32 - 0.5 * width) / width,
#         im: 2.0 * (y as f32 - 0.5 * height) / height,
#     };
#
#     let mut i = 0;
#     for t in 0..max_iter {
#         if z.norm() >= 2.0 {
#             break;
#         }
#         z = z * z + c;
#         i = t;
#     }
#     i
# }
#
# // 规格 RGB 颜色值范围内的强度值
# fn normalize(color: f32, factor: f32) -> u8 {
#     ((color * factor).powf(0.8) * 255.) as u8
# }

fn main() -> Result<()> {
    let (width, height) = (1920, 1080);
    let mut img = ImageBuffer::new(width, height);
    let iterations = 300;

    let c = Complex::new(-0.8, 0.156);

    let pool = ThreadPool::new(num_cpus::get());
    let (tx, rx) = channel();

    for y in 0..height {
        let tx = tx.clone();
        pool.execute(move || for x in 0..width {
                         let i = julia(c, x, y, width, height, iterations);
                         let pixel = wavelength_to_rgb(380 + i * 400 / iterations);
                         tx.send((x, y, pixel)).expect("Could not send data!");
                     });
    }

    for _ in 0..(width * height) {
        let (x, y, pixel) = rx.recv()?;
        img.put_pixel(x, y, pixel);
    }
    let _ = img.save("output.png")?;
    Ok(())
}
```

[`ImageBuffer::new`]: https://docs.rs/image/*/image/struct.ImageBuffer.html#method.new
[`ImageBuffer::put_pixel`]: https://docs.rs/image/*/image/struct.ImageBuffer.html#method.put_pixel
[`ImageBuffer::save`]: https://docs.rs/image/*/image/struct.ImageBuffer.html#method.save
[`mpsc::channel`]: https://doc.rust-lang.org/std/sync/mpsc/fn.channel.html
[`num_cpus::get`]: https://docs.rs/num_cpus/*/num_cpus/fn.get.html
[`Receiver::recv`]: https://doc.rust-lang.org/std/sync/mpsc/struct.Receiver.html#method.recv
[`Rgb::from_channels`]: https://docs.rs/image/*/image/struct.Rgb.html#method.from_channels
[`ThreadPool`]: https://docs.rs/threadpool/*/threadpool/struct.ThreadPool.html
[`ThreadPool::execute`]: https://docs.rs/threadpool/*/threadpool/struct.ThreadPool.html#method.execute
[Julia set]: https://en.wikipedia.org/wiki/Julia_set
