# Substrate Frameless Node Template

A stripped down version of the [node template](https://github.com/substrate-developer-hub/substrate-node-template), ready for hackin'.

## TODO

1. Make it a flipper: it stores one bool value, and per each transaction, flips that value.
1. Make it an adder: it stores one u32 value, and each transaction specifies a number, which is added to this value and stored onchain again.
1. Extend your transaction so that the runtime can be either an adder or a multiplier.
1. Add a kill-switch to this runtime. Look into well_known_keys to see which key you have to wipe.
1. Make this runtime upgradable! The upgrade operation can simply be protected by a "password" as you don't have any notion of accounts yet.
1. Add a notion of accounts and nonces and signatures.
1. Add a notion of balances
1. Write a custom runtime API, and try to call it over the RPC.
1. Implement a tx-pool api, implement tipping, priority, longevity etc.

## Build

The `cargo run` command will perform an initial build. Use the following command to build the node
without launching it:

```sh
cargo b -r
```

### Embedded CLI Docs

Once the project has been built, the following command can be used to explore all parameters and
subcommands:

```sh
./target/release/node-template -h
```

### Single-Node Development Chain

This command will start the single-node development chain with non-persistent state:

```bash
./target/release/node-template --dev
```

Purge the development chain's state:

```bash
./target/release/node-template purge-chain --dev
```

Start the development chain with detailed logging:

```bash
RUST_BACKTRACE=1 ./target/release/node-template -ldebug --dev
```

In case of being interested in maintaining the chain's state between runs a base path must be added:

```bash
// Create a folder to use as the db base path
$ mkdir my-chain-state

// Use of that folder to store the chain state
$ ./target/release/node-template --dev --base-path ./my-chain-state/

// Check the folder structure created inside the base path after running the chain
$ ls ./my-chain-state
chains
$ ls ./my-chain-state/chains/
dev
$ ls ./my-chain-state/chains/dev
db keystore network
```

## commands

To build a new runtime version (for upgrade experimentation)

```zsh
CARGO_TARGET_DIR=./my-target cargo build --release
```

To upgrade the current node's runtime with a new one as created above:

```zsh
cargo run -p runtime-updater --release
```

Submit an extrinsic:

```zsh
curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8" -d   '{
        "jsonrpc":"2.0",
        "id":1,
        "method":"author_submitExtrinsic",
        "params": ["0x0102000000"]
}'
```

Read a storage value:

```zsh
curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8" -d   '{
        "jsonrpc":"2.0",
        "id":1,
        "method":"state_getStorage",
        "params": ["0x616363"]
}'
```

Get runtime version:

```zsh
curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8" -d   '{
        "jsonrpc":"2.0",
        "id":1,
        "method":"state_getRuntimeVersion"
}'
```
