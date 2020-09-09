## 创建并行的管道

<!--
> [concurrency/thread/crossbeam-complex.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/concurrency/thread/crossbeam-complex.md)
> <br />
> commit 4789410122527d556a3520204e5bab637326e13f - 2020.07.31
-->

[![crossbeam-badge]][crossbeam] [![cat-concurrency-badge]][cat-concurrency]

下面的实例使用 [crossbeam] 和 [crossbeam-channel] 两个 crate 创建了一个并行的管道，与 ZeroMQ [指南][guide] 中所描述的类似：管道有一个数据源和一个数据接收器，数据在从源到接收器的过程中由两个工作线程并行处理。

我们使用容量由 [`crossbeam_channel::bounded`] 分配的有界信道。生产者必须在它自己的线程上，因为它产生的消息比工作线程处理它们的速度快（因为工作线程休眠了半秒）——这意味着生产者将在对 `[crossbeam_channel::Sender::send`] 调用时阻塞半秒，直到其中一个工作线程对信道中的数据处理完毕。也请注意，信道中的数据由最先接收它的任何工作线程调用，因此每个消息都传递给单个工作线程，而不是传递给两个工作线程。

通过迭代器 [`crossbeam_channel::Receiver::iter`] 方法从信道读取数据，这将会造成阻塞，要么等待新消息，要么直到信道关闭。因为信道是在 [`crossbeam::scope`] 范围内创建的，我们必须通过 `drop` 手动关闭它们，以防止整个程序阻塞工作线程的 for 循环。你可以将对 `drop` 的调用视作不再发送消息的信号。

```rust
extern crate crossbeam;
extern crate crossbeam_channel;

use std::thread;
use std::time::Duration;
use crossbeam_channel::bounded;

fn main() {
    let (snd1, rcv1) = bounded(1);
    let (snd2, rcv2) = bounded(1);
    let n_msgs = 4;
    let n_workers = 2;

    crossbeam::scope(|s| {
        // 生产者线程
        s.spawn(|_| {
            for i in 0..n_msgs {
                snd1.send(i).unwrap();
                println!("Source sent {}", i);
            }
            // 关闭信道 —— 这是退出的必要条件
            // for 巡海在工作线程中
            drop(snd1);
        });

        // 由 2 个县城并行处理
        for _ in 0..n_workers {
            // 从数据源发送数据到接收器，接收器接收数据
            let (sendr, recvr) = (snd2.clone(), rcv1.clone());
            // 在不同的线程中衍生工人
            s.spawn(move |_| {
            thread::sleep(Duration::from_millis(500));
                // 接收数据，直到信道关闭前
                for msg in recvr.iter() {
                    println!("Worker {:?} received {}.",
                             thread::current().id(), msg);
                    sendr.send(msg * 2).unwrap();
                }
            });
        }
        // 关闭信道，否则接收器不会关闭
        // 退出 for 循坏
        drop(snd2);

        // 接收器
        for msg in rcv2.iter() {
            println!("Sink received {}", msg);
        }
    }).unwrap();
}
```

[`crossbeam::scope`]: https://docs.rs/crossbeam/*/crossbeam/fn.scope.html
[crossbeam-channel]: https://docs.rs/crossbeam-channel/*/crossbeam_channel/index.html
[`crossbeam_channel::bounded`]: https://docs.rs/crossbeam-channel/*/crossbeam_channel/fn.bounded.html
[`crossbeam_channel::Receiver::iter`]: https://docs.rs/crossbeam-channel/*/crossbeam_channel/struct.Receiver.html#method.iter
[`crossbeam_channel::Sender::send`]: https://docs.rs/crossbeam-channel/*/crossbeam_channel/struct.Sender.html#method.send
[guide]: http://zguide.zeromq.org/page:all#Divide-and-Conquer
