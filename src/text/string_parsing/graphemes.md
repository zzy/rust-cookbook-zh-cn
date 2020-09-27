## 收集 Unicode 字符

<!--
> [text/string_parsing/graphemes.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/text/string_parsing/graphemes.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![unicode-segmentation-badge]][`unicode-segmentation`] [![cat-text-processing-badge]][cat-text-processing]

使用 [`unicode-segmentation`] crate 中的 [`UnicodeSegmentation::graphemes`] 函数，从 UTF-8 字符串中收集个别的 Unicode 字符。

```rust,edition2018
use unicode_segmentation::UnicodeSegmentation;

fn main() {
    let name = "José Guimarães\r\n";
    let graphemes = UnicodeSegmentation::graphemes(name, true)
    	.collect::<Vec<&str>>();
	assert_eq!(graphemes[3], "é");
}
```

[`UnicodeSegmentation::graphemes`]: https://docs.rs/unicode-segmentation/*/unicode_segmentation/trait.UnicodeSegmentation.html#tymethod.graphemes
[`unicode-segmentation`]: https://docs.rs/unicode-segmentation/1.2.1/unicode_segmentation/
