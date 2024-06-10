u<div align="center">
🔌 ✨

</div>

<h1 align="center">
  hyperlocal
</h1>

<p align="center">
   <a href="https://github.com/hyperium/hyper">Hyper</a> client and server bindings for <a href="https://github.com/tokio-rs/tokio/tree/master/tokio-net/src/uds/">Unix domain sockets</a>
</p>

<div align="center">
  <a alt="GitHub Actions" href="https://github.com/softprops/hyperlocal/actions">
    <img src="https://github.com/softprops/hyperlocal/workflows/Main/badge.svg"/>
  </a>
  <a alt="crates.io" href="https://crates.io/crates/hyperlocal">
    <img src="https://img.shields.io/crates/v/hyperlocal.svg?logo=rust"/>
  </a>
  <a alt="docs.rs" href="http://docs.rs/hyperlocal">
    <img src="https://docs.rs/hyperlocal/badge.svg"/>
  </a>
  <a alt="latest docs" href="https://softprops.github.io/hyperlocal">
   <img src="https://img.shields.io/badge/docs-latest-green.svg"/>
  </a>
  <a alt="license" href="LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-brightgreen.svg"/>
  </a>
</div>

<br />

Hyper is a rock solid [Rust](https://www.rust-lang.org/) HTTP client and server toolkit.
[Unix domain sockets](https://en.wikipedia.org/wiki/Unix_domain_socket) provide a mechanism
for host-local interprocess communication. `hyperlocal` builds on and complements Hyper's
interfaces for building Unix domain socket HTTP clients.

This is useful for accessing HTTP interfaces exposed via a Unix daemons.
Examples of Unix daemons that provide this kind of host local interface include
[Docker](https://docs.docker.com/engine/misc/), a process container manager.

## Installation

Add the following to your `Cargo.toml` file

```toml
[dependencies]
hyperlocal = "0.9"
```

## Usage

### Servers

A typical server can be built by creating a `tokio::net::UnixListener` and accepting connections in a loop using
`hyper::service::service_fn` to create a request/response processing function, and connecting the `UnixStream` to it
using `hyper::server::conn::http1::Builder::new().serve_connection()`.

An example is at [examples/server.rs](./examples/server.rs), runnable via `cargo run --example server`

To test that your server is working you can use an out-of-the-box tool like `curl`

```sh
$ curl --unix-socket /tmp/hyperlocal.sock localhost

It's a Unix system. I know this.
```

Note that `hyperlocal` is not required to build a server, though `hyper` and `tokio` are both used in the example.

### Clients

`hyperlocal` provides bindings for writing unix domain socket based HTTP clients the `Client` interface from the
`hyper-utils` crate.

An example is at [examples/client.rs](./examples/client.rs), runnable via `cargo run --example client`

Hyper's client interface makes it easy to send typical HTTP methods like `GET`, `POST`, `DELETE` with factory
methods, `get`, `post`, `delete`, etc. These require an argument that can be tranformed into a `hyper::Uri`.

Since Unix domain sockets aren't represented with hostnames that resolve to ip addresses coupled with network ports,
your standard over the counter URL string won't do. Instead, use a `hyperlocal::Uri`, which represents both file path to the domain
socket and the resource URI path and query string.

## Recent Releases of `hyperlocal`

### 0.9

Supports `hyper 1.x` by providing a `tower` service `UnixConnector` and an
extension method `hyper_util::client::legacy::Client::unix()` to create a
client. 

The server extension method `bind_unix` was removed since there is no longer
an equivalent to `hyper::Server`.

### 0.8

Supports `hyper 0.14` and provided extensions to both hyper's `Client` and `Server` via traits.

---

Doug Tangren (softprops) 2015-2020
