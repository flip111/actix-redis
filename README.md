# Actix redis [![Build Status](https://travis-ci.org/actix/actix-redis.svg?branch=master)](https://travis-ci.org/actix/actix-redis) [![codecov](https://codecov.io/gh/actix/actix-redis/branch/master/graph/badge.svg)](https://codecov.io/gh/actix/actix-redis) [![crates.io](http://meritbadge.herokuapp.com/actix-redis)](https://crates.io/crates/actix-redis) 

Redis integration for actix framework.

## Documentation

* [API Documentation](http://actix.github.io/actix-redis/actix_redis/)
* [Chat on gitter](https://gitter.im/actix/actix)
* Cargo package: [actix-redis](https://crates.io/crates/actix-redis)
* Minimum supported Rust version: 1.39 or later


## Redis session backend

Use redis as session storage.

You need to pass an address of the redis server and random value to the
constructor of `RedisSessionBackend`. This is private key for cookie session,
When this value is changed, all session data is lost.

Note that whatever you write into your session is visible by the user (but not modifiable).

Constructor panics if key length is less than 32 bytes.

```rust
use actix_web::{App, HttpServer, web, middleware};
use actix_web::middleware::session::SessionStorage;
use actix_redis::RedisSessionBackend;

#[actix_rt::main]
async fn main() -> std::io::Result {
    HttpServer::new(|| App::new()
        // enable logger
        .middleware(middleware::Logger::default())
        // cookie session middleware
        .middleware(SessionStorage::new(
            RedisSessionBackend::new("127.0.0.1:6379", &[0; 32])
        ))
        // register simple route, handle all methods
        .service(web::resource("/").to(index))
    )
    .bind("0.0.0.0:8080")?
    .start()
    .await
}
```

## License

This project is licensed under either of

* Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0))
* MIT license ([LICENSE-MIT](LICENSE-MIT) or [http://opensource.org/licenses/MIT](http://opensource.org/licenses/MIT))

at your option.

## Code of Conduct

Contribution to the actix-redis crate is organized under the terms of the
Contributor Covenant, the maintainer of actix-redis, @fafhrd91, promises to
intervene to uphold that code of conduct.
