## 检查给定版本是否为预发布版本

<!--
> [development_tools/versioning/semver-prerelease.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/versioning/semver-prerelease.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![semver-badge]][semver] [![cat-config-badge]][cat-config]

给定两个版本，使用 [`is_prerelease`] 断言一个是预发布，另一个不是。

```rust,edition2018
use semver::{Version, SemVerError};

fn main() -> Result<(), SemVerError> {
    let version_1 = Version::parse("1.0.0-alpha")?;
    let version_2 = Version::parse("1.0.0")?;

    assert!(version_1.is_prerelease());
    assert!(!version_2.is_prerelease());

    Ok(())
}
```

[`is_prerelease`]: https://docs.rs/semver/*/semver/struct.Version.html#method.is_prerelease
