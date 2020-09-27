## 客户端

<!--
> [web/clients.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web/clients.md)
> <br />
> commit dd4efa8dcd8e611326caa01c08db8f227aa909d6 - 2020.06.07
-->

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [发出 HTTP GET 请求][ex-url-basic] | [![reqwest-badge]][reqwest] | [![cat-net-badge]][cat-net] |
| [为 REST 请求设置自定义消息头和 URL 参数][ex-rest-custom-params] | [![reqwest-badge]][reqwest] [![hyper-badge]][hyper] [![url-badge]][url] [![cat-net-badge]][cat-net] |
| [查询 GitHub API][ex-rest-get] | [![reqwest-badge]][reqwest] [![serde-badge]][serde] | [![cat-net-badge]][cat-net] [![cat-encoding-badge]][cat-encoding] |
| [检查 API 资源是否存在][ex-rest-head] | [![reqwest-badge]][reqwest] | [![cat-net-badge]][cat-net] |
| [使用 GitHub API 创建和删除 Gist][ex-rest-post] | [![reqwest-badge]][reqwest] [![serde-badge]][serde] | [![cat-net-badge]][cat-net] [![cat-encoding-badge]][cat-encoding] |
| [使用 RESTful API 分页][ex-paginated-api] | [![reqwest-badge]][reqwest] [![serde-badge]][serde] | [![cat-net-badge]][cat-net] [![cat-encoding-badge]][cat-encoding] |
| [处理速率限制 API][ex-handle-rate-limited-api] | [![reqwest-badge]][reqwest] [![hyper-badge]][hyper] [![cat-net-badge]][cat-net] |
| [下载文件到临时目录][ex-url-download] | [![reqwest-badge]][reqwest] [![tempdir-badge]][tempdir] | [![cat-net-badge]][cat-net] [![cat-filesystem-badge]][cat-filesystem] |
| [使用 HTTP range 请求头进行部分下载][ex-progress-with-range] | [![reqwest-badge]][reqwest] | [![cat-net-badge]][cat-net] |
| [POST 文件到 paste-rs][ex-file-post] | [![reqwest-badge]][reqwest] | [![cat-net-badge]][cat-net] |

[ex-url-basic]: /web/clients/requests.md#发出-http-get-请求
[ex-rest-custom-params]: /web/clients/requests.md#为-rest-请求设置自定义消息头和-url-参数
[ex-rest-get]: /web/clients/apis.md#查询-github-api
[ex-rest-head]: /web/clients/apis.md#检查-api-资源是否存在
[ex-rest-post]: /web/clients/apis.md#使用-github-api-创建和删除-gist
[ex-paginated-api]: /web/clients/apis.md#使用-restful-api-分页
[ex-handle-rate-limited-api]: /web/clients/apis.md#处理速率限制-api
[ex-url-download]: /web/clients/download.md#下载文件到临时目录
[ex-progress-with-range]: /web/clients/download.md#使用-http-range-请求头进行部分下载
[ex-file-post]: /web/clients/download.md#post-文件到-paste-rs

{{#include ../links.md}}
