# 并发/并行

<!--
> [concurrency.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/concurrency.md)
> <br />
> commit 3c32c84475a2d99aa1d1b2d5d2e480aeada47293 - 2020.06.07
-->

| 实例名称 | Crates | 类别 |
|--------|--------|------------|
| [生成短期线程][ex-crossbeam-spawn] | [![crossbeam-badge]][crossbeam] | [![cat-concurrency-badge]][cat-concurrency] |
| [创建并发的数据管道][ex-crossbeam-pipeline] | [![crossbeam-badge]][crossbeam] | [![cat-concurrency-badge]][cat-concurrency] |
| [在两个线程间传递数据][ex-crossbeam-spsc] | [![crossbeam-badge]][crossbeam] | [![cat-concurrency-badge]][cat-concurrency] |
| [保持全局可变状态][ex-global-mut-state] | [![lazy_static-badge]][lazy_static] | [![cat-rust-patterns-badge]][cat-rust-patterns] |
| [对所有 iso 文件的 SHA256 值并发求和][ex-threadpool-walk]  | [![threadpool-badge]][threadpool] [![walkdir-badge]][walkdir] [![num_cpus-badge]][num_cpus] [![ring-badge]][ring] | [![cat-concurrency-badge]][cat-concurrency][![cat-filesystem-badge]][cat-filesystem] |
| [将绘制分形的线程分派到线程池][ex-threadpool-fractal] | [![threadpool-badge]][threadpool] [![num-badge]][num] [![num_cpus-badge]][num_cpus] [![image-badge]][image] | [![cat-concurrency-badge]][cat-concurrency][![cat-science-badge]][cat-science][![cat-rendering-badge]][cat-rendering] |
| [并行改变数组中元素][ex-rayon-iter-mut] | [![rayon-badge]][rayon] | [![cat-concurrency-badge]][cat-concurrency] |
| [并行测试集合中任意或所有的元素是否匹配给定断言][ex-rayon-any-all] | [![rayon-badge]][rayon] | [![cat-concurrency-badge]][cat-concurrency] |
| [使用给定断言并行搜索项][ex-rayon-parallel-search] | [![rayon-badge]][rayon] | [![cat-concurrency-badge]][cat-concurrency] |
| [对 vector 并行排序][ex-rayon-parallel-sort] | [![rayon-badge]][rayon] [![rand-badge]][rand] | [![cat-concurrency-badge]][cat-concurrency] |
| [Map-reduce 并行计算][ex-rayon-map-reduce] | [![rayon-badge]][rayon] | [![cat-concurrency-badge]][cat-concurrency] |
| [并行生成 jpg 缩略图][ex-rayon-thumbnails] | [![rayon-badge]][rayon] [![glob-badge]][glob] [![image-badge]][image] | [![cat-concurrency-badge]][cat-concurrency][![cat-filesystem-badge]][cat-filesystem] |


[ex-crossbeam-spawn]: concurrency/threads.md#生成短期线程
[ex-crossbeam-pipeline]: concurrency/threads.md#创建并发的数据管道
[ex-crossbeam-spsc]: concurrency/threads.md#在两个线程间传递数据
[ex-global-mut-state]: concurrency/threads.md#保持全局可变状态
[ex-threadpool-walk]: concurrency/threads.md#对所有-iso-文件的-sha256-值并发求和
[ex-threadpool-fractal]: concurrency/threads.md#将绘制分形的线程分派到线程池
[ex-rayon-iter-mut]: concurrency/parallel.md#并行改变数组中元素
[ex-rayon-any-all]: concurrency/parallel.md#并行测试集合中任意或所有的元素是否匹配给定断言
[ex-rayon-parallel-search]: concurrency/parallel.md#使用给定断言并行搜索项
[ex-rayon-parallel-sort]: concurrency/parallel.md#对-vector-并行排序
[ex-rayon-map-reduce]: concurrency/parallel.md#map-reduce-并行计算
[ex-rayon-thumbnails]: concurrency/parallel.md#并行生成-jpg-缩略图

{{#include links.md}}
