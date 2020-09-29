## 从文件名获取 MIME 类型

<!--
> [web/mime/filename.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/mime/filename.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![mime-badge]][mime] [![cat-encoding-badge]][cat-encoding]

下面的实例展示如何使用 [mime] crate 从给定的文件名返回正确的 MIME 类型。程序将检查文件扩展名并与已知的 MIME 类型列表匹配，返回值为 [`mime:Mime`]。

```rust,edition2018
use mime::Mime;

fn find_mimetype (filename : &String) -> Mime{

    let parts : Vec<&str> = filename.split('.').collect();

    let res = match parts.last() {
            Some(v) =>
                match *v {
                    "png" => mime::IMAGE_PNG,
                    "jpg" => mime::IMAGE_JPEG,
                    "json" => mime::APPLICATION_JSON,
                    &_ => mime::TEXT_PLAIN,
                },
            None => mime::TEXT_PLAIN,
        };
    return res;
}

fn main() {
    let filenames = vec!("foobar.jpg", "foo.bar", "foobar.png");
    for file in filenames {
	    let mime = find_mimetype(&file.to_owned());
	 	println!("MIME for {}: {}", file, mime);
	 }

}
```

[`mime:Mime`]: https://docs.rs/mime/*/mime/struct.Mime.html
