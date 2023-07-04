# Setting up the environment

To work with CosmWasm a smart contract, you will need Rust installed on your
machine. If you do not already have it installed, you can find installation
instructions on [the Rust website](https://www.rust-lang.org/tools/install).

We assume you are working with a stable Rust version in this book.

Additionally, you will need the Wasm Rust compiler backend installed to build
Wasm binaries. To install it, run:

```
rustup target add wasm32-unknown-unknown
```

Optionally, if you want to try out your contracts on a testnet, you will need a
[wasmd](https://github.com/CosmWasm/wasmd) binary. We shall focus on testing
contracts with Rust unit testing utility throughout the book, so it is not
required to follow on testnet. However, seeing the product working in a real-world
environment may be nice.

To install `wasmd`, first install the [golang](https://github.com/golang/go/wiki#working-with-go). Then
clone the `wasmd` and install it:

```
$ git clone git@github.com:CosmWasm/wasmd.git
$ cd ./wasmd
$ make install
```

Also, to be able to upload Rust Wasm Contracts into the blockchain, you will need
to install [docker](https://www.docker.com/). To minimize your contract sizes,
it will be required to run CosmWasm Rust Optimizer; without it, more complex
contracts might exceed the size limit.

## cosmwasm-check utility

An additional helpful tool for building smart contracts is the `cosmwasm-check`[utility](https://github.com/CosmWasm/cosmwasm/tree/main/packages/check). It allows you to check if
the Wasm binary is a properly-formed smart contract ready to upload into the blockchain.
You can install it using cargo:

```
$ cargo install cosmwasm-check
```

If the installation succeeds, you should be able to execute the utility from the command line.

```
$ cosmwasm-check --version
Contract checking 1.2.3
```

## Verifying the installation

To guarantee you are ready to build your smart contracts, you need to make sure you can build examples.
Checkout the [cw-plus](https://github.com/CosmWasm/cw-plus) repository and run the testing command in
its folder:

```
$ git clone git@github.com:CosmWasm/cw-plus.git
$ cd ./cw-plus
cw-plus $ cargo test
```

You should see that everything in the repository gets compiled, and all tests pass. 

`cw-plus` is a great place to find example contracts - look for them in `contracts` directory. The
repository is maintained by CosmWasm creators, so contracts in there should follow good practices.

To verify the `cosmwasm-check` utility, first, you need to build a smart contract. Go to some contract directory, for example, `contracts/cw1-whitelist`, and call `cargo wasm`:

```
cw-plus $ cd contracts/cw1-whitelist
cw-plus/contracts/cw1-whitelist $ cargo wasm
```

You should be able to find your output binary in the `target/wasm32-unknown-unknown/release/`
of the root repo directory - not in the contract directory itself! Now you can check if the contract
passes validation:

```
cw-plus/contracts/cw1-whitelist $ cosmwasm-check ../../target/wasm32-unknown-unknown/release/cw1_whitelist.wasm
Available capabilities: {"iterator", "cosmwasm_1_1", "cosmwasm_1_2", "stargate", "staking"}

../../target/wasm32-unknown-unknown/release/cw1_whitelist.wasm: pass

All contracts (1) passed checks!
```
