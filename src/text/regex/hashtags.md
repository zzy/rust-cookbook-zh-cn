## 从文本提取标签元素唯一的列表

<!--
> [text/regex/hashtags.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/text/regex/hashtags.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![regex-badge]][regex] [![lazy_static-badge]][lazy_static] [![cat-text-processing-badge]][cat-text-processing]

本实例展示从文本中提取、排序和去除标签列表的重复元素。

这里给出的标签正则表达式只捕获以字母开头的拉丁语标签，完整的 [twitter 标签正则表达式][twitter hashtag regex]要复杂得多。

```rust,edition2018
use lazy_static::lazy_static;

use regex::Regex;
use std::collections::HashSet;

fn extract_hashtags(text: &str) -> HashSet<&str> {
    lazy_static! {
        static ref HASHTAG_REGEX : Regex = Regex::new(
                r"\#[a-zA-Z][0-9a-zA-Z_]*"
            ).unwrap();
    }
    HASHTAG_REGEX.find_iter(text).map(|mat| mat.as_str()).collect()
}

fn main() {
    let tweet = "Hey #world, I just got my new #dog, say hello to Till. #dog #forever #2 #_ ";
    let tags = extract_hashtags(tweet);
    assert!(tags.contains("#dog") && tags.contains("#forever") && tags.contains("#world"));
    assert_eq!(tags.len(), 3);
}
```

[twitter hashtag regex]: https://github.com/twitter/twitter-text/blob/c9fc09782efe59af4ee82855768cfaf36273e170/java/src/com/twitter/Regex.java#L255
