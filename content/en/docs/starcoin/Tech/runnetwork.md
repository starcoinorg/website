---
title: Run/Join Network
weight: 3
---

# Run Local Network

`starcoin` is used to start a local network and a local blockchain on your computer. Running a local network makes it easier to test and debug your code changes. You can use the CLI command dev to compile, publish, and execute Move Intermediate Representation (IR) programs on your local cluster of nodes. 

## Usage

`starcoin` [FLAGS] [OPTIONS] [SUBCOMMAND]

FLAGS:
- --disable-file-log Disable std error log output
- --disable-seed Disable seed for seed node


OPTIONS:
- --seed config seed node address manually
- --dev-period in dev network, the block mined frequence, default is 0.
- --net network name ,it should be one of dev/halley/proxima/main
- -s sync mode , it should be full or fast

SUBCOMMAND:
- console Run node background, after node started ,start cli console
- help  Prints this message or the help of the given subcommand(s)


the following command could start new dev node with 10 second per block:

```
cargo run --bin starcoin -- -n dev --dev-peroid 10 
```

after this command,you cound find node address in log or std output ,it could like:

```
Self address is: /ip4/127.0.0.1/tcp/59476/p2p/12D3KooWPePRG6BDdjgtEYmPDxNyJfMWpQ1Rwgefuz9eqksLfxJb
```

then you could setup another node by this command:

```
cargo run --bin starcoin -- -n dev --dev-peroid 10 --seed /ip4/127.0.0.1/tcp/59476/p2p/12D3KooWPePRG6BDdjgtEYmPDxNyJfMWpQ1Rwgefuz9eqksLfxJb

```

You could use subcommand console to start cli console:

```
cargo run --bin starcoin -- -n dev --dev-peroid 10 console
```

repeat these steps , you cloud get multi node local dev network.

# Join Halley/Proxima Network

You could use such command to join halley/proxima network:
```
cargo run --bin starcoin -- -n dev --dev-peroid 10 console
```