## 监听未使用的 TCP/IP 端口

<!--
> [net/server/listen-unused.md](https://github.com/rust-lang-nursery/rust-cookbook/blob/master/src/net/server/listen-unused.md)
> <br />
> commit b61c8e588ad8445de36cd5f28e99232b5f858a41 - 2020.06.01
-->

[![std-badge]][std] [![cat-net-badge]][cat-net]

在本例中，程序将监听显示在控制台上的端口，直到一个请求被发出。当将端口设置为 0 时，`SocketAddrV4` 会分配一个随机端口。

```rust,edition2018,no_run
use std::net::{SocketAddrV4, Ipv4Addr, TcpListener};
use std::io::{Read, Error};

fn main() -> Result<(), Error> {
    let loopback = Ipv4Addr::new(127, 0, 0, 1);
    let socket = SocketAddrV4::new(loopback, 0);
    let listener = TcpListener::bind(socket)?;
    let port = listener.local_addr()?;
    println!("Listening on {}, access this port to end the program", port);
    let (mut tcp_stream, addr) = listener.accept()?; // 阻塞，直到被请求
    println!("Connection received! {:?} is sending data.", addr);
    let mut input = String::new();
    let _ = tcp_stream.read_to_string(&mut input)?;
    println!("{:?} says {}", addr, input);
    Ok(())
}
```
