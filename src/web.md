# Web 编程

<!--
> [web.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/web.md)
> <br />
> commit dd4efa8dcd8e611326caa01c08db8f227aa909d6 - 2020.06.07
-->

## 抓取网页

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [从 HTML 网页中提取所有链接][ex-extract-links-webpage] | [![reqwest-badge]][reqwest] [![select-badge]][select] | [![cat-net-badge]][cat-net] |
| [检查网页死链][ex-check-broken-links] | [![reqwest-badge]][reqwest] [![select-badge]][select] [![url-badge]][url] | [![cat-net-badge]][cat-net] |
| [从 MediaWiki 标记页面提取所有唯一性链接][ex-extract-mediawiki-links] | [![reqwest-badge]][reqwest] [![regex-badge]][regex] | [![cat-net-badge]][cat-net] |

[ex-extract-links-webpage]: web/scraping.md#从-html-网页中提取所有链接
[ex-check-broken-links]: web/scraping.md#检查网页死链
[ex-extract-mediawiki-links]: web/scraping.md#从-mediawiki-标记页面提取所有唯一性链接

## URL

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [解析 URL 字符串为 `Url` 类型][ex-url-parse] | [![url-badge]][url] | [![cat-net-badge]][cat-net] |
| [通过移除路径段创建基本 URL][ex-url-base] | [![url-badge]][url] | [![cat-net-badge]][cat-net] |
| [从基本 URL 创建新 URLs][ex-url-new-from-base] | [![url-badge]][url] | [![cat-net-badge]][cat-net] |
| [提取 URL 源（scheme/host/port）][ex-url-origin] | [![url-badge]][url] | [![cat-net-badge]][cat-net] |
| [从 URL 移除片段标识符和查询对][ex-url-rm-frag] | [![url-badge]][url] | [![cat-net-badge]][cat-net] |

[ex-url-parse]: web/url.md#解析-url-字符串为-url-类型
[ex-url-base]: web/url.md#通过移除路径段创建基本-url
[ex-url-new-from-base]: web/url.md#从基本-url-创建新-urls
[ex-url-origin]: web/url.md#提取-url-源-scheme-host-port
[ex-url-rm-frag]: web/url.md#从-url-移除片段标识符和查询对

## 媒体类型（MIME）

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [Get MIME type from string][ex-mime-from-string] | [![mime-badge]][mime] | [![cat-encoding-badge]][cat-encoding] |
| [Get MIME type from filename][ex-mime-from-filename] | [![mime-badge]][mime] | [![cat-encoding-badge]][cat-encoding] |
| [Parse the MIME type of a HTTP response][ex-http-response-mime-type] | [![mime-badge]][mime] [![reqwest-badge]][reqwest] | [![cat-net-badge]][cat-net] [![cat-encoding-badge]][cat-encoding] |

[ex-mime-from-string]: web/mime.md#get-mime-type-from-string
[ex-mime-from-filename]: web/mime.md#get-mime-type-from-filename
[ex-http-response-mime-type]: web/mime.md#parse-the-mime-type-of-a-http-response

{{#include web/clients.md}}

{{#include links.md}}
