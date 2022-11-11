# mobilecoin-local-network

## Starting The Network

To start the mobilecoin network, run ```docker compose up -d``` from the root directory. This should start a docker cluster with 3 Consensus Nodes and Fog, exposed on the following ports:

Node 0: 3200
Node 1: 3201
Node 2: 3202
Fog: 8200

There are LocalStack S3 buckets running for each of the nodes ledger distributions at ```http://localhost:4563/node-<NODE NUM>-ledger```


## Connecting To The Network

In order to connect to this network, when building your service make sure to set the following environment variables

```sh
MC_SEED=e9b4b31bf94b43c969fa33590b1356cc4854008220154050ab95802bb9f770b9
IAS_MODE=DEV
SGX_MODE=SW
```

and also make sure to use the included consensus-enclave.css and ingest-enclave.css files.

### Example - Full Service

#### Build

```sh
MC_SEED=e9b4b31bf94b43c969fa33590b1356cc4854008220154050ab95802bb9f770b9 \
IAS_MODE=DEV \
SGX_MODE=SW \
CONSENSUS_ENCLAVE_CSS=$(pwd)/consensus-enclave.css \
cargo build --release
```

#### Run

```sh
mkdir -p wallet-db/
./full-service \
    --wallet-db wallet-db/wallet.db \
    --ledger-db ledger-db/ \
    --peer insecure-mc://localhost:3200 \
    --peer insecure-mc://localhost:3201 \
    --tx-source-url http://localhost:4566/node-0-ledger \
    --tx-source-url http://localhost:4566/node-1-ledger \
    --fog-ingest-enclave-css $(pwd)/ingest-enclave.css \
    --chain-id local
```

