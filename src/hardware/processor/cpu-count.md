## 检查逻辑 cpu 内核的数量

<!--
> [hardware/processor/cpu-count.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/hardware/processor/cpu-count.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![num_cpus-badge]][num_cpus] [![cat-hardware-support-badge]][cat-hardware-support]

使用 [`num_cpus::get`] 显示当前机器中的逻辑 CPU 内核的数量。

```rust,edition2018
fn main() {
    println!("Number of logical cores is {}", num_cpus::get());
}
```
