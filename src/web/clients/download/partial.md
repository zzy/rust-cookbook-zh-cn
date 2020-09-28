## 使用 HTTP range 请求头进行部分下载

<!--
> [web/clients/download/partial.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/clients/download/partial.md)
> <br />
> commit dd4efa8dcd8e611326caa01c08db8f227aa909d6 - 2020.06.07
-->

[![reqwest-badge]][reqwest] [![cat-net-badge]][cat-net]

使用 [`reqwest::blocking::Client::head`] 获取响应的[消息主体的大小][Content-Length]（即消息主体内容长度）。

然后，使用 [`reqwest::blocking::Client::get`] 下载 10240 字节的内容，同时打印进度消息。本实例使用同步的 reqwest 模块，[消息范围][Range]标头指定响应的消息块大小和位置。

[RFC7233][HTTP Range RFC7233] 中定义了[消息范围][Range]标头。

> 译者注：RFC（Request For Comments）是一系列以编号排定的文件。文件收集了有关互联网相关信息，以及 UNIX 和互联网社区的软件文件。

```rust,edition2018,no_run
use error_chain::error_chain;
use reqwest::header::{HeaderValue, CONTENT_LENGTH, RANGE};
use reqwest::StatusCode;
use std::fs::File;
use std::str::FromStr;

error_chain! {
    foreign_links {
        Io(std::io::Error);
        Reqwest(reqwest::Error);
        Header(reqwest::header::ToStrError);
    }
}

struct PartialRangeIter {
  start: u64,
  end: u64,
  buffer_size: u32,
}

impl PartialRangeIter {
  pub fn new(start: u64, end: u64, buffer_size: u32) -> Result<Self> {
    if buffer_size == 0 {
      Err("invalid buffer_size, give a value greater than zero.")?;
    }
    Ok(PartialRangeIter {
      start,
      end,
      buffer_size,
    })
  }
}

impl Iterator for PartialRangeIter {
  type Item = HeaderValue;
  fn next(&mut self) -> Option<Self::Item> {
    if self.start > self.end {
      None
    } else {
      let prev_start = self.start;
      self.start += std::cmp::min(self.buffer_size as u64, self.end - self.start + 1);
      Some(HeaderValue::from_str(&format!("bytes={}-{}", prev_start, self.start - 1)).expect("string provided by format!"))
    }
  }
}

fn main() -> Result<()> {
  let url = "https://httpbin.org/range/102400?duration=2";
  const CHUNK_SIZE: u32 = 10240;
    
  let client = reqwest::blocking::Client::new();
  let response = client.head(url).send()?;
  let length = response
    .headers()
    .get(CONTENT_LENGTH)
    .ok_or("response doesn't include the content length")?;
  let length = u64::from_str(length.to_str()?).map_err(|_| "invalid Content-Length header")?;
    
  let mut output_file = File::create("download.bin")?;
    
  println!("starting download...");
  for range in PartialRangeIter::new(0, length - 1, CHUNK_SIZE)? {
    println!("range {:?}", range);
    let mut response = client.get(url).header(RANGE, range).send()?;
    
    let status = response.status();
    if !(status == StatusCode::OK || status == StatusCode::PARTIAL_CONTENT) {
      error_chain::bail!("Unexpected server response: {}", status)
    }
    std::io::copy(&mut response, &mut output_file)?;
  }
    
  let content = response.text()?;
  std::io::copy(&mut content.as_bytes(), &mut output_file)?;

  println!("Finished with success!");
  Ok(())
}
```

[`reqwest::blocking::Client::get`]: https://docs.rs/reqwest/*/reqwest/blocking/struct.Client.html#method.get
[`reqwest::blocking::Client::head`]: https://docs.rs/reqwest/*/reqwest/blocking/struct.Client.html#method.head
[Content-Length]: https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Length
[Range]: https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Range

[HTTP Range RFC7233]: https://tools.ietf.org/html/rfc7233#section-3.1
