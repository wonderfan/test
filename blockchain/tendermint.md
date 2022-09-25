# Tendermint Load Test

## Installation

```bash

git clone https://github.com/informalsystems/tm-load-test.git
git clone -b v0.34.11 https://github.com/tendermint/tendermint.git

```

## Setting Modification

```

pprof_laddr = ":6060"

- GOMAXPROCS=2

ports:
    - "26656-26657:26656-26657"
    - "6060:6060"


```

## Test Service

```sh

cd tendermint
make build
make localnet-stop
make localnet-start

cd tm-load-test
make build

./build/tm-load-test -c 20 -T 120 -r 1000 -s 250 \
    --broadcast-tx-method async \
    --endpoints ws://127.0.0.1:26657/websocket,ws://127.0.0.1:26662/websocket,ws://127.0.0.1:26664/websocket,ws://127.0.0.1:26660/websocket

./build/tm-load-test -c 4 -T 120 -r 200 -s 250 \
    --broadcast-tx-method sync \
    --endpoints ws://127.0.0.1:26657/websocket,ws://127.0.0.1:26660/websocket,ws://127.0.0.1:26662/websocket,ws://127.0.0.1:26664/websocket    

```

## Debug Files

```

curl http://localhost:6060/debug/pprof/trace?seconds=120 > trace.out

go tool pprof http://127.0.0.1:6060/debug/pprof/profile

```