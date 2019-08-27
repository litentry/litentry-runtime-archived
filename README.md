# litentry

## Introduction

### Abstract

The identity runtime protocol abstract all the persons and devices authorization scenario, includes three basic definitions:

1. Identity Registry: a pre-defined key pair to present a person or a device.

2. Token: a non-fungible token issued by the identity.

3. Data: the related identity data, like identity meta information and token specification. 

![Identity Runtime Protocol](https://static.wixstatic.com/media/760d3a_5c9818f59cd74e63a9cacd72d51db660~mv2.png/v1/fill/w_823,h_462,al_c,q_90/760d3a_5c9818f59cd74e63a9cacd72d51db660~mv2.webp)

### Examples:

##### Person ID chain: 

* Identity Registry: crypto key pairs bind to national identity number;

* Token: a token created by the person, which could be used to validate the age;

* Data: the data of the person, e.g. birth date and place, gender, etc.

##### Lock Chain:

* Locks Registry: crypto key pairs bind to the smart IoT lock;

* Token:  a token created by the lock, which could be used as an entry key.

* Data: the definition of the token, e.g how many times could one token be used.

### Features

##### Cross Chain Validation

Such a protocol could be easily combined for the Polkadot cross chain feature, thus could realize many new ideas:

For example, the entry token on the lock chain need to be combined together with the personal ID chain, the entry token is only valid if the user's age is more than 18. The other personal data will not be exposed at the same time.

##### Runtime Specific Data Operation:

An IoT device manufacturer could have its own data operation functions, for example, it may harvest the data from all the temperature sensors and pay DOT to the sensor owners. 

##### Data Linking

The large data bind to the identity owner, i.e. person or devices, will be saved into an IPFS data server, the entry hash link will bind to their identity. The benefit of it is once the data updates, the old link will be also disabled. In order to always get the available data, others need to ask permission from the user. 

A new SRML-based Substrate node, ready for hacking.

## Building

Install Rust:

```bash
curl https://sh.rustup.rs -sSf | sh
```

Install required tools:

```bash
./scripts/init.sh
```

Build the WebAssembly binary:

```bash
./scripts/build.sh
```

Build all native code:

```bash
cargo build
```

## Run

You can start a development chain with:

```bash
cargo run -- --dev
```

Detailed logs may be shown by running the node with the following environment variables set: `RUST_LOG=debug RUST_BACKTRACE=1 cargo run -- --dev`.

If you want to see the multi-node consensus algorithm in action locally, then you can create a local testnet with two validator nodes for Alice and Bob, who are the initial authorities of the genesis chain that have been endowed with testnet units. Give each node a name and expose them so they are listed on the Polkadot [telemetry site](https://telemetry.polkadot.io/#/Local%20Testnet). You'll need two terminal windows open.

We'll start Alice's substrate node first on default TCP port 30333 with her chain database stored locally at `/tmp/alice`. The bootnode ID of her node is `QmQZ8TjTqeDj3ciwr93EJ95hxfDsb9pEYDizUAbWpigtQN`, which is generated from the `--node-key` value that we specify below:

```bash
cargo run -- \
  --base-path /tmp/alice \
  --chain=local \
  --alice \
  --node-key 0000000000000000000000000000000000000000000000000000000000000001 \
  --telemetry-url ws://telemetry.polkadot.io:1024 \
  --validator
```

In the second terminal, we'll start Bob's substrate node on a different TCP port of 30334, and with his chain database stored locally at `/tmp/bob`. We'll specify a value for the `--bootnodes` option that will connect his node to Alice's bootnode ID on TCP port 30333:

```bash
cargo run -- \
  --base-path /tmp/bob \
  --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/QmQZ8TjTqeDj3ciwr93EJ95hxfDsb9pEYDizUAbWpigtQN \
  --chain=local \
  --bob \
  --port 30334 \
  --telemetry-url ws://telemetry.polkadot.io:1024 \
  --validator
```

Additional CLI usage options are available and may be shown by running `cargo run -- --help`.
