## 处理速率受限 API

<!--
> [web/clients/api/rate-limited.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/clients/api/rate-limited.md)
> <br />
> commit 203b1085212a7b857d9a29bdc6a763515e77e0f9 - 2020.06.08
-->

[![reqwest-badge]][reqwest] [![hyper-badge]][hyper] [![cat-net-badge]][cat-net]

此实例使用 [GitHub API - 速率限制](https://docs.github.com/cn/free-pro-team@latest/rest/reference/rate-limit)展示如何处理远程服务器错误。本实例使用 [`hyper::header!`] 宏来解析响应头并检查 [`reqwest::StatusCode::Forbidden`]。如果响应超过速率限制，则将等待并重试。

```rust,edition2018,no_run
# use error_chain::error_chain;

use std::time::{Duration, UNIX_EPOCH};
use std::thread;
use reqwest::StatusCode;
#
# error_chain! {
#    foreign_links {
#        Io(std::io::Error);
#        Time(std::time::SystemTimeError);
#        Reqwest(reqwest::Error);
#    }
# }

header! { (XRateLimitLimit, "X-RateLimit-Limit") => [usize] }
header! { (XRateLimitRemaining, "X-RateLimit-Remaining") => [usize] }
header! { (XRateLimitReset, "X-RateLimit-Reset") => [u64] }

fn main() -> Result<()> {
    loop {
        let url = "https://api.github.com/users/rust-lang-nursery ";
        let client = reqwest::Client::new();
        let response = client.get(url).send()?;

        let rate_limit = response.headers().get::<XRateLimitLimit>().ok_or(
            "response doesn't include the expected X-RateLimit-Limit header",
        )?;

        let rate_remaining = response.headers().get::<XRateLimitRemaining>().ok_or(
            "response doesn't include the expected X-RateLimit-Remaining header",
        )?;

        let rate_reset_at = response.headers().get::<XRateLimitReset>().ok_or(
            "response doesn't include the expected X-RateLimit-Reset header",
        )?;

        let rate_reset_within = Duration::from_secs(**rate_reset_at) - UNIX_EPOCH.elapsed()?;

        if response.status() == StatusCode::Forbidden && **rate_remaining == 0 {
            println!("Sleeping for {} seconds.", rate_reset_within.as_secs());
            thread::sleep(rate_reset_within);
            return main();
        } else {
            println!(
                "Rate limit is currently {}/{}, the reset of this limit will be within {} seconds.",
                **rate_remaining,
                **rate_limit,
                rate_reset_within.as_secs(),
            );
            break;
        }
    }
    Ok(())
}
```

[`hyper::header!`]: https://doc.servo.org/hyper/header/index.html#defining-custom-headers
[`reqwest::StatusCode::Forbidden`]: https://docs.rs/reqwest/*/reqwest/struct.StatusCode.html#associatedconstant.FORBIDDEN
