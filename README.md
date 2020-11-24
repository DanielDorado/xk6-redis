# xk6-redis

This is a Redis client library for [k6](https://github.com/loadimpact/k6),
implemented as an extension using the [xk6](https://github.com/k6io/xk6) system.

| :exclamation: This is a proof of concept, isn't supported by the k6 team, and may break in the future. USE AT YOUR OWN RISK! |
|------|

## Build

To build a `k6` binary with this extension, first ensure you have the prerequisites:

- [Go toolchain](https://go101.org/article/go-toolchain.html)
- Git

Then:

1. Install `xk6`:
  ```shell
  go get -u github.com/k6io/xk6/cmd/xk6
  ```

2. Build the binary:
  ```shell
  xk6 build v0.29.0 --with github.com/k6io/xk6-redis
  ```

## Example test script

```javascript
// test.js
import redis from 'k6/x/redis';

const client = redis.newClient({
  addr: 'localhost:6379',
  password: '',
  db: 0,
});

export default function () {
  client.set('mykey', 'myvalue', 0);
  console.log(`mykey => ${client.get('mykey')}`);
}
```

Result output:

```shell
$ ./k6 run test.js

          /\      |‾‾| /‾‾/   /‾‾/
     /\  /  \     |  |/  /   /  /
    /  \/    \    |     (   /   ‾‾\
   /          \   |  |\  \ |  (‾)  |
  / __________ \  |__| \__\ \_____/ .io

  execution: local
     script: test.js
     output: -

  scenarios: (100.00%) 1 scenario, 1 max VUs, 10m30s max duration (incl. graceful stop):
           * default: 1 iterations for each of 1 VUs (maxDuration: 10m0s, gracefulStop: 30s)

INFO[0000] mykey => myvalue                              source=console

running (00m00.0s), 0/1 VUs, 1 complete and 0 interrupted iterations
default ✓ [======================================] 1 VUs  00m00.0s/10m0s  1/1 iters, 1 per VU

    data_received........: 0 B 0 B/s
    data_sent............: 0 B 0 B/s
    iteration_duration...: avg=834.68µs min=834.68µs med=834.68µs max=834.68µs p(90)=834.68µs p(95)=834.68µs
    iterations...........: 1   54.622575/s

```
