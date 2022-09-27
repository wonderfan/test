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

./build/tm-load-test -c 4 -T 120 -r 50 -s 250 \
    --broadcast-tx-method sync \
    --endpoints ws://127.0.0.1:26657/websocket,ws://127.0.0.1:26660/websocket,ws://127.0.0.1:26662/websocket,ws://127.0.0.1:26664/websocket    

```

## Debug Files

```

curl http://localhost:6060/debug/pprof/trace?seconds=120 > trace.out

go tool pprof http://127.0.0.1:6060/debug/pprof/profile

```


## Cosmos-SDK

```bash

git clone -b v0.43.0 https://gitee.com/wonderfan/cosmos-sdk.git

cd cosmos-sdk

make localnet-start

./simd keys add tom --keyring-backend test --home node3/simd

while true; do
    ./simd tx bank send cosmos1xer4l09p9zxgy0p386cf2fkzd3f75hh6hrunnh cosmos18kuk50gdwmuc6m85jgj7x7ecnx6e0p6ygzj2qz 1testtoken --chain-id chain-R1lnrj --keyring-backend test --home node3/simd --yes
    ./simd tx bank send cosmos1l4hjgmvgs773fempd842666nmd9q87pyuzu0lc cosmos18kuk50gdwmuc6m85jgj7x7ecnx6e0p6ygzj2qz 1testtoken --chain-id chain-R1lnrj --keyring-backend test --home node0/simd --yes
    ./simd tx bank send cosmos15fqlvjlqdvllxwce3sevefkj87dz4600asvu4q cosmos18kuk50gdwmuc6m85jgj7x7ecnx6e0p6ygzj2qz 1testtoken --chain-id chain-R1lnrj --keyring-backend test --home node1/simd --yes
    ./simd tx bank send cosmos1ldqfgffa2sas3a275ud726w0jy6kdr03sh9saa cosmos18kuk50gdwmuc6m85jgj7x7ecnx6e0p6ygzj2qz 1testtoken --chain-id chain-R1lnrj --keyring-backend test --home node2/simd --yes
    sleep 2s
done

./simd keys list --home node2/simd --keyring-backend test

./simd query bank balances cosmos1ldqfgffa2sas3a275ud726w0jy6kdr03sh9saa


ssh-keygen -C gitpod -q

for i in {0..3}; do
  hcd unsafe-reset-all --home node$i
done


while true; do
  hcd tx bank send erhai1e5huuqsz4zedzd4xwgxt3a6muxfc0kjz3pp805 erhai12rsdhrt4ngg9dnxm2utdvqcfjm3h3wht7lzvhl 1token --chain-id erhai --keyring-backend test --home node0 --yes -b block
done

hcd keys add jimmy --home node0

while true; do
  hcd tx bank send erhai12rsdhrt4ngg9dnxm2utdvqcfjm3h3wht7lzvhl erhai1lmu0he6rj29k9q8qau4h74zlhk8969aj9pdnpx 1token --chain-id erhai --keyring-backend test --home node0 --yes -b block
done

hcd keys add lily --home node0


while true; do
  hcd tx bank send erhai1lmu0he6rj29k9q8qau4h74zlhk8969aj9pdnpx erhai1gkd48sx9kxqspfh77j9cquk840x66dffg5h96l  1token --chain-id erhai --keyring-backend test --home node0 --yes -b block
done

hcd tx permission account approve erhai1gkd48sx9kxqspfh77j9cquk840x66dffg5h96l ROLE_USER --chain-id erhai --home node0 --yes -b block --from admin

while true; do
  hcd tx bank send erhai1gkd48sx9kxqspfh77j9cquk840x66dffg5h96l erhai1e5huuqsz4zedzd4xwgxt3a6muxfc0kjz3pp805  1token --chain-id erhai --keyring-backend test --home node0 --yes -b block
done

hcd keys add john --home node0 --output=json 

```

## Permissioned Chain Based on Cosmos-SDK

- [ledger](https://github.com/zigbee-alliance/distributed-compliance-ledger)
- [meta](https://github.com/davebryson/menta)
- [pki](https://github.com/hashcloak/katzenmint-pki)
- [steelcerts](https://github.com/olaeseane/steelcerts)

## dmesg command

The dmesg command lets you peer into the hidden world of the Linux startup processes. Review and monitor hardware device and driver messages from the kernel’s own ring buffer with “the fault finder’s friend.”

A ring buffer is a memory space reserved for messages. It is simple in design, and of a fixed size. When it is full, newer messages overwrite the oldest messages. Conceptually it can be thought of as a “circular buffer.”

The kernel ring buffer stores information such as the initialization messages of device drivers, messages from hardware, and messages from kernel modules. Because it contains these low-level startup messages, the ring buffer is a good place to start an investigation into hardware errors or other startup issues.

