# Build

cargo concordium build --out dist/module.wasm.v1 --schema-out dist/schema.bin

# Deploy

concordium-client module deploy dist/module.wasm.v1 --sender <YOUR-ADDRESS> --name counter --grpc-port 10001
concordium-client module deploy dist/module.wasm.v1 --sender 3AiAikC2v4H3Rv4L58oHvdX5N8LJoarsQwN3G7oNS7BoFsmt7N --name fr --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# Initialize the contract

concordium-client contract init <YOUR-MODULE-HASH> --sender <YOUR-ADDRESS> --energy 30000 --contract counter --grpc-port 10001

concordium-client contract init 1ebdda8d37d922af03a320b5e0c5ceab8c534de871939712adaff4b8e680fa68 --sender 3AiAikC2v4H3Rv4L58oHvdX5N8LJoarsQwN3G7oNS7BoFsmt7N --energy 30000 --contract CIS2-Fractionalizer --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# Increment

concordium-client contract update <YOUR-CONTRACT-INSTANCE> --entrypoint mint --parameter-json <PATH-TO-JSON> --schema dist/schema.bin --sender <YOUR-ADDRESS> --energy 6000 --grpc-port 10001

# View state

concordium-client contract invoke <YOUR-CONTRACT-INSTANCE> --entrypoint view --schema dist/schema.bin --grpc-port 10001

##### 3A NFT-Contract: 2755 TokenId: 01

- Perquisites

  - A CIS2 Compatible Contract Instance with atleast 1 minted token (Please see [cis2-multi instructions](./cis2-multi.README.md) to do the same)

- Initialize Smart Contract

  ```bash
  concordium-client --grpc-ip $GRPC_IP --grpc-port $GRPC_PORT contract init <MODULE_REF> --contract CIS2-Fractionalizer --sender $ACCOUNT --energy 3000
  ```

- Transfer CIS2 Token to the Fractionalizer Contract

  ```
  concordium-client contract update 2755 --entrypoint transfer --parameter-json cis2-fractionalizer/cis2-multi-transfer.json --schema dist/schema.bin --sender 3AiAikC2v4H3Rv4L58oHvdX5N8LJoarsQwN3G7oNS7BoFsmt7N --energy 6000 --grpc-port 10000 --grpc-ip node.testnet.concordium.com
  ```

- Mint

  ```bash
  concordium-client --grpc-ip $GRPC_IP --grpc-port $GRPC_PORT contract update 2163 --entrypoint mint --parameter-json ../sample-artifacts/cis2-fractionalizer/mint.json --schema ../cis2-fractionalizer/schema.bin --sender $ACCOUNT --energy 6000
  ```

- Burn

  We transfer some part of the amount of the minted tokens back to the contract address to burn the tokens

  ```bash
  concordium-client --grpc-ip $GRPC_IP --grpc-port $GRPC_PORT contract update 2163 --entrypoint transfer --parameter-json ../sample-artifacts/cis2-fractionalizer/burn-20.json --schema ../cis2-fractionalizer/schema.bin --sender $ACCOUNT --energy 6000
  ```

  ```bash
  concordium-client --grpc-ip $GRPC_IP --grpc-port $GRPC_PORT contract update 2163 --entrypoint transfer --parameter-json ../sample-artifacts/cis2-fractionalizer/burn-80.json --schema ../cis2-fractionalizer/schema.bin --sender $ACCOUNT --energy 6000
  ```

- [View CIS2-Multi](./cis2-multi.README.md)
