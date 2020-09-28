## 下载文件到临时目录

<!--
> [web/clients/download/basic.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/clients/download/basic.md)
> <br />
> commit dd4efa8dcd8e611326caa01c08db8f227aa909d6 - 2020.06.07
-->

[![reqwest-badge]][reqwest] [![tempdir-badge]][tempdir] [![cat-net-badge]][cat-net] [![cat-filesystem-badge]][cat-filesystem]

使用 [`tempfile::Builder`] 创建一个临时目录，并使用 [`reqwest::get`] 通过 HTTP 协议异步下载文件。

使用 [`Response::url`] 方法内部的 [`tempdir()`] 方法获取文件名字，使用  [`File`]  结构体创建目标文件，并使用 [`io::copy`] 将下载的数据复制到文件中。程序退出时，会自动删除临时目录。

```rust,edition2018,no_run
use error_chain::error_chain;
use std::io::copy;
use std::fs::File;
use tempfile::Builder;

error_chain! {
     foreign_links {
         Io(std::io::Error);
         HttpRequest(reqwest::Error);
     }
}

#[tokio::main]
async fn main() -> Result<()> {
    let tmp_dir = Builder::new().prefix("example").tempdir()?;
    let target = "https://www.rust-lang.org/logos/rust-logo-512x512.png";
    let response = reqwest::get(target).await?;

    let mut dest = {
        let fname = response
            .url()
            .path_segments()
            .and_then(|segments| segments.last())
            .and_then(|name| if name.is_empty() { None } else { Some(name) })
            .unwrap_or("tmp.bin");

        println!("file to download: '{}'", fname);
        let fname = tmp_dir.path().join(fname);
        println!("will be located under: '{:?}'", fname);
        File::create(fname)?
    };
    let content =  response.text().await?;
    copy(&mut content.as_bytes(), &mut dest)?;
    Ok(())
}
```

[`File`]: https://doc.rust-lang.org/std/fs/struct.File.html
[`io::copy`]: https://doc.rust-lang.org/std/io/fn.copy.html
[`reqwest::get`]: https://docs.rs/reqwest/*/reqwest/fn.get.html
[`Response::url`]: https://docs.rs/reqwest/*/reqwest/struct.Response.html#method.url
[`tempfile::Builder`]: https://docs.rs/tempfile/*/tempfile/struct.Builder.html
[`tempdir()`]: https://docs.rs/tempfile/3.1.0/tempfile/struct.Builder.html#method.tempdir
