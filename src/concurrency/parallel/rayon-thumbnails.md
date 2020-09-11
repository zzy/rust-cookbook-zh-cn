## 并行生成 jpg 缩略图

<!--
> [concurrency/parallel/rayon-thumbnails.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/concurrency/parallel/rayon-thumbnails.md)
> <br />
> commit 203b1085212a7b857d9a29bdc6a763515e77e0f9 - 2020.06.08
-->

[![rayon-badge]][rayon] [![glob-badge]][glob] [![image-badge]][image] [![cat-concurrency-badge]][cat-concurrency] [![cat-filesystem-badge]][cat-filesystem]

本实例为当前目录中的所有 .jpg 图像文件生成缩略图，然后将生成的缩略图保存在一个名为 `thumbnails` 的新文件夹中。

[`glob::glob_with`] 在当前目录中查找 jpeg 图像文件，`rayon` 通过 [`par_iter`] 方法调用 [`DynamicImage::resize`]，并行地调整图像大小。

```rust,edition2018,no_run
# use error_chain::error_chain;

use std::path::Path;
use std::fs::create_dir_all;

# use error_chain::ChainedError;
use glob::{glob_with, MatchOptions};
use image::{FilterType, ImageError};
use rayon::prelude::*;

# error_chain! {
#     foreign_links {
#         Image(ImageError);
#         Io(std::io::Error);
#         Glob(glob::PatternError);
#     }
# }

fn main() -> Result<()> {
    let options: MatchOptions = Default::default();
    let files: Vec<_> = glob_with("*.jpg", options)?
        .filter_map(|x| x.ok())
        .collect();

    if files.len() == 0 {
        error_chain::bail!("No .jpg files found in current directory");
    }

    let thumb_dir = "thumbnails";
    create_dir_all(thumb_dir)?;

    println!("Saving {} thumbnails into '{}'...", files.len(), thumb_dir);

    let image_failures: Vec<_> = files
        .par_iter()
        .map(|path| {
            make_thumbnail(path, thumb_dir, 300)
                .map_err(|e| e.chain_err(|| path.display().to_string()))
        })
        .filter_map(|x| x.err())
        .collect();

    image_failures.iter().for_each(|x| println!("{}", x.display_chain()));

    println!("{} thumbnails saved successfully", files.len() - image_failures.len());
    Ok(())
}

fn make_thumbnail<PA, PB>(original: PA, thumb_dir: PB, longest_edge: u32) -> Result<()>
where
    PA: AsRef<Path>,
    PB: AsRef<Path>,
{
    let img = image::open(original.as_ref())?;
    let file_path = thumb_dir.as_ref().join(original);

    Ok(img.resize(longest_edge, longest_edge, FilterType::Nearest)
        .save(file_path)?)
}
```

[`glob::glob_with`]: https://docs.rs/glob/*/glob/fn.glob_with.html
[`par_iter`]: https://docs.rs/rayon/*/rayon/iter/trait.IntoParallelRefIterator.html#tymethod.par_iter
[`DynamicImage::resize`]: https://docs.rs/image/*/image/enum.DynamicImage.html#method.resize
