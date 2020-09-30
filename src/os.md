# 操作系统

<!--
> [os.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/os.md)
> <br />
> commit bba147f18531934c904b1c5afaed3e6550b1c1c0 - 2020.06.14
-->

## 外部命令

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [运行外部命令并处理 stdout][ex-parse-subprocess-output] | [![regex-badge]][regex] | [![cat-os-badge]][cat-os] [![cat-text-processing-badge]][cat-text-processing] |
| [运行传递 stdin 的外部命令，并检查错误代码][ex-parse-subprocess-input] | [![regex-badge]][regex] | [![cat-os-badge]][cat-os] [![cat-text-processing-badge]][cat-text-processing] |
| [运行管道传输的外部命令][ex-run-piped-external-commands] | [![std-badge]][std] | [![cat-os-badge]][cat-os] |
| [将子进程的 stdout 和 stderr 重定向到同一个文件][ex-redirect-stdout-stderr-same-file] | [![std-badge]][std] | [![cat-os-badge]][cat-os] |
| [持续处理子进程的输出][ex-continuous-process-output] | [![std-badge]][std] | [![cat-os-badge]][cat-os][![cat-text-processing-badge]][cat-text-processing] |
| [读取环境变量][ex-read-env-variable] | [![std-badge]][std] | [![cat-os-badge]][cat-os] |


[ex-parse-subprocess-output]: os/external.md#运行外部命令并处理-stdout
[ex-parse-subprocess-input]: os/external.md#运行传递-stdin-的外部命令并检查错误代码
[ex-run-piped-external-commands]: os/external.md#运行管道传输的外部命令
[ex-redirect-stdout-stderr-same-file]: os/external.md#将子进程的-stdout-和-stderr-重定向到同一个文件
[ex-continuous-process-output]: os/external.md#持续处理子进程的输出
[ex-read-env-variable]: os/external.md#读取环境变量

{{#include links.md}}
