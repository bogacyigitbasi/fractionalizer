**With this repo you will find schema files as it's needed to call transfer function**
Basically you need to transfer minted CIS-2 token to the contract first, it will be locked as collateral then you will be able to mint fractions.

# Build

cargo concordium build --out dist/module.wasm.v1 --schema-out dist/schema.bin

# Deploy

concordium-client module deploy dist/module.wasm.v1 --sender <YOUR-ADDRESS> --name CIS2-Fractionalizer --grpc-port 10000 --grpc-ip node.testnet.concordium.com

concordium-client module deploy dist/module.wasm.v1 --sender 3AiAikC2v4H3Rv4L58oHvdX5N8LJoarsQwN3G7oNS7BoFsmt7N --name CIS2-Fractionalizer --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# Initialize the contract

concordium-client contract init bdc82a84a5262997dc2d4da0ed8a9a59a2166d3b185bc44927d2c1ac64e317cc --sender 3AiAikC2v4H3Rv4L58oHvdX5N8LJoarsQwN3G7oNS7BoFsmt7N --energy 30000 --contract CIS2-Fractionalizer --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# View state

concordium-client contract invoke <YOUR-CONTRACT-INSTANCE> --entrypoint view --schema dist/schema.bin --grpc-port 10001

concordium-client contract invoke 3132 --entrypoint view --schema dist/schema.bin --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# Transfer

concordium-client contract update <YOUR-TOKEN-CONTRACT-INSTANCE> --entrypoint transfer --parameter-json cis2-fractionalizer/cis2-multi-transfer.json --schema multi/dist/schema.bin --sender <YOUR-ADDRESS> --energy 6000 --grpc-port 10000 --grpc-ip node.testnet.concordium.com

concordium-client contract update 2844 --entrypoint transfer --parameter-json cis2-fractionalizer/cis2-multi-transfer.json --schema multi/dist/schema.bin --sender 3AiAikC2v4H3Rv4L58oHvdX5N8LJoarsQwN3G7oNS7BoFsmt7N --energy 6000 --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# Mint

concordium-client contract update <YOUR-CONTRACT-INSTANCE> --entrypoint mint --parameter-json cis2-fractionalizer/mint.json --schema dist/schema.bin --sender <YOUR-ADDRESS> --energy 6000 --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# Burn

We transfer some part of the amount of the minted tokens back to the contract address to burn the tokens

concordium-client contract update 2995 --entrypoint transfer --parameter-json cis2-fractionalizer/burn-20.json --schema dist/schema.bin --sender 3AiAikC2v4H3Rv4L58oHvdX5N8LJoarsQwN3G7oNS7BoFsmt7N --energy 6000 --grpc-port 10000 --grpc-ip node.testnet.concordium.com

concordium-client contract update 2995 --entrypoint transfer --parameter-json cis2-fractionalizer/transfer-account.json --schema dist/schema.bin --sender 3AiAikC2v4H3Rv4L58oHvdX5N8LJoarsQwN3G7oNS7BoFsmt7N --energy 6000 --grpc-port 10000 --grpc-ip node.testnet.concordium.com

concordium-client contract update <YOUR-CONTRACT-INSTANCE> --entrypoint transfer --parameter-json cis2-fractionalizer/burn-20.json --schema dist/schema.bin --sender <YOUR-ADDRESS> --energy 6000 --grpc-port 10000 --grpc-ip node.testnet.concordium.com

concordium-client contract update <YOUR-CONTRACT-INSTANCE> --entrypoint transfer --parameter-json cis2-fractionalizer/burn-80.json --schema dist/schema.bin --sender <YOUR-ADDRESS> --energy 6000 --grpc-port 10000 --grpc-ip node.testnet.concordium.com
