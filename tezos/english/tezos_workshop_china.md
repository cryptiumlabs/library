# Tezos Workshop

The following instructions explain how to install all the required Tezos software. Furthermore, it runs through a simple contract creation and delegation example. Lastly, it deploys and interacts with a smart contract written in Michelson.

This guide has been tested with:
* Ubuntu 16.04
* Ubuntu 18.04
* macOS - 10.13

## Setup Phase

### System Dependencies

#### Ubuntu

If you are on Ubuntu 16.04, please run the following:
```
sudo add-apt-repository ppa:ansible/bubblewrap
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get update
```

For both 16.04 and 18.04 run:

```
sudo apt-get install -y patch unzip make gcc m4 git g++ aspcud bubblewrap curl bzip2 rsync libev-dev libgmp-dev pkg-config libhidapi-dev
```

#### macOS

There are no system dependencies that have to be installed.

### Installing opam, the ocaml package manager

Start by installing opam.
```
sh <(curl -sL https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh)
```

Then initialise opam and activate it.
```
opam init --compiler=4.06.1
eval $(opam env)
```

### Install the Tezos daemons and clients

Clone the Tezos source code:
```
git clone -b betanet https://gitlab.com/tezos/tezos.git
```

Build and install the Tezos binaries:
```
cd tezos
make build-deps
eval $(opam env)
make
```

## Beginning Phase

You have the latest version of all the Tezos daemons and clients installed and are ready to interact with the global Tezos network.

In this phase we will use the most basic commands to get started.

### Launch and sync

The first step is to generate a PoW nonce to be used in the P2P layer to prevent spam.
```
./tezos-node identity generate 26.
```

Now you are ready to launch your own Tezos node.
```
./tezos-node run --rpc-addr 127.0.0.1:8732 --connections 10
```

Syncing will take some time, probably more than 4 hours. You can check on the progress like this:
```
./tezos-client bootstrapped
```

We are running a fully synced Tezos node and if you don't want to wait 4 hours you can use that one.
```
./tezos-client -A betanet.tezos.cryptium.ch bootstrapped
```

