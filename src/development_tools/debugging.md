## 调试工具

<!--
> [development_tools/debugging.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/development_tools/debugging.md)
> <br />
> commit 97dabe59ae705bf6a2aaebbcd1d189ec2a83f98b - 2018.07.11
-->

## 日志信息

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [记录调试信息到控制台][ex-log-debug] | [![log-badge]][log] [![env_logger-badge]][env_logger] | [![cat-debugging-badge]][cat-debugging] |
| [记录错误信息到控制台][ex-log-error] | [![log-badge]][log] [![env_logger-badge]][env_logger] | [![cat-debugging-badge]][cat-debugging] |
| [记录信息时，用标准输出 stdout 替换标准错误 stderr][ex-log-stdout] | [![log-badge]][log] [![env_logger-badge]][env_logger] | [![cat-debugging-badge]][cat-debugging] |
| [使用自定义日志记录器记录信息][ex-log-custom-logger] | [![log-badge]][log] | [![cat-debugging-badge]][cat-debugging] |
| [记录到 Unix 系统日志][ex-log-syslog] | [![log-badge]][log] [![syslog-badge]][syslog] | [![cat-debugging-badge]][cat-debugging] |

## 日志配置

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [启用每个模块的日志级别][ex-log-mod] | [![log-badge]][log] [![env_logger-badge]][env_logger] | [![cat-debugging-badge]][cat-debugging] |
| [用自定义环境变量设置日志记录][ex-log-env-variable] | [![log-badge]][log] [![env_logger-badge]][env_logger] | [![cat-debugging-badge]][cat-debugging] |
| [在日志信息中包含时间戳][ex-log-timestamp] | [![log-badge]][log] [![env_logger-badge]][env_logger] [![chrono-badge]][chrono] | [![cat-debugging-badge]][cat-debugging] |
| [将信息记录到自定义位置][ex-log-custom] | [![log-badge]][log] [![log4rs-badge]][log4rs] | [![cat-debugging-badge]][cat-debugging] |

[ex-log-debug]: /development_tools/debugging/log.md#记录调试信息到控制台
[ex-log-error]: /development_tools/debugging/log.md#记录错误信息到控制台
[ex-log-stdout]: /development_tools/debugging/log.md#记录信息时用标准输出-stdout-替换标准错误-stderr
[ex-log-custom-logger]: /development_tools/debugging/log.md#使用自定义日志记录器记录信息
[ex-log-syslog]: /development_tools/debugging/log.md#记录到-unix-系统日志

[ex-log-mod]: /development_tools/debugging/config_log.md#启用每个模块的日志级别
[ex-log-env-variable]: /development_tools/debugging/config_log.md#用自定义环境变量设置日志记录
[ex-log-timestamp]: /development_tools/debugging/config_log.md#在日志信息中包含时间戳
[ex-log-custom]: /development_tools/debugging/config_log.md#将信息记录到自定义位置

{{#include ../links.md}}
