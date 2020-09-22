## 解析复杂的版本字符串

<!--
> [development_tools/versioning/semver-complex.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/versioning/semver-complex.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![semver-badge]][semver] [![cat-config-badge]][cat-config]

使用 [`Version::parse`] 从复杂的版本字符串构造语义化版本 [`semver::Version`]。该字符串包含[语义化版本控制规范][Semantic Versioning Specification]中定义的预发布和构建元数据。

需要注意的是：根据[语义化版本控制规范][Semantic Versioning Specification]，构建元数据是虽然被解析，但在比较版本时不考虑。换句话说，即使两个版本的构建字符串不同，但它们的版本可能是相等的。

```rust,edition2018
use semver::{Identifier, Version, SemVerError};

fn main() -> Result<(), SemVerError> {
    let version_str = "1.0.49-125+g72ee7853";
    let parsed_version = Version::parse(version_str)?;

    assert_eq!(
        parsed_version,
        Version {
            major: 1,
            minor: 0,
            patch: 49,
            pre: vec![Identifier::Numeric(125)],
            build: vec![],
        }
    );
    assert_eq!(
        parsed_version.build,
        vec![Identifier::AlphaNumeric(String::from("g72ee7853"))]
    );

    let serialized_version = parsed_version.to_string();
    assert_eq!(&serialized_version, version_str);

    Ok(())
}
```

[`semver::Version`]: https://docs.rs/semver/*/semver/struct.Version.html
[`Version::parse`]: https://docs.rs/semver/*/semver/struct.Version.html#method.parse

[Semantic Versioning Specification]: http://semver.org/
