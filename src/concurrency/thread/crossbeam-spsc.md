# 在两个线程间传递数据

<!--
> [concurrency/thread/crossbeam-spsc.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/concurrency/thread/crossbeam-spsc.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![crossbeam-badge]][crossbeam] [![cat-concurrency-badge]][cat-concurrency]

这个实例示范了在单生产者、单消费者（SPSC）环境中使用 [crossbeam-channel]。我们构建的[生成短期线程][ex-crossbeam-spawn]实例中，使用 [`crossbeam::scope`] 和 [`Scope::spawn`] 来管理生产者线程。在两个线程之间，使用 [`crossbeam_channel::unbounded`] 信道交换数据，这意味着可存储消息的数量没有限制。生产者线程在消息之间休眠半秒。

```rust,edition2018

use std::{thread, time};
use crossbeam_channel::unbounded;

fn main() {
    let (snd, rcv) = unbounded();
    let n_msgs = 5;
    crossbeam::scope(|s| {
        s.spawn(|_| {
            for i in 0..n_msgs {
                snd.send(i).unwrap();
                thread::sleep(time::Duration::from_millis(100));
            }
        });
    }).unwrap();
    for _ in 0..n_msgs {
        let msg = rcv.recv().unwrap();
        println!("Received {}", msg);
    }
}
```

[crossbeam-channel]: https://docs.rs/crate/crossbeam-channel/
[ex-crossbeam-spawn]: concurrency/threads.md#生成短期线程
[`crossbeam::scope`]: https://docs.rs/crossbeam/*/crossbeam/fn.scope.html
[`Scope::spawn`]: https://docs.rs/crossbeam/*/crossbeam/thread/struct.Scope.html#method.spawn
[`crossbeam_channel::unbounded`]: https://docs.rs/crossbeam-channel/*/crossbeam_channel/fn.unbounded.html
