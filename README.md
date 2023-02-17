**With this repo you will find schema files as it's needed to call transfer function**
Basically you need to transfer minted CIS-2 token to the contract first, it will be locked as collateral then you will be able to mint fractions.

# Build

cargo concordium build --out dist/module.wasm.v1 --schema-out dist/schema.bin

# Deploy

concordium-client module deploy dist/module.wasm.v1 --sender YOUR-ADDRESS --name CIS2-Fractionalizer --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# Initialize the contract

concordium-client contract init MODULE-HASH --sender YOUR-ADDRESS --energy 30000 --contract CIS2-Fractionalizer --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# View state

concordium-client contract invoke YOUR-CONTRACT-INSTANCE --entrypoint view --schema dist/schema.bin --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# Transfer

concordium-client contract update YOUR-TOKEN-CONTRACT-INSTANCE --entrypoint transfer --parameter-json cis2-fractionalizer/cis2-multi-transfer.json --schema multi/dist/schema.bin --sender YOUR-ADDRESS --energy 6000 --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# Mint

concordium-client contract update YOUR-CONTRACT-INSTANCE --entrypoint mint --parameter-json cis2-fractionalizer/mint.json --schema dist/schema.bin --sender YOUR-ADDRESS --energy 6000 --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# Burn - transfer back to the contract

We transfer some part of the amount of the minted tokens back to the contract address to burn the tokens

concordium-client contract update YOUR-CONTRACT-INSTANCE --entrypoint transfer --parameter-json cis2-fractionalizer/burn-20.json --schema dist/schema.bin --sender YOUR-ADDRESS --energy 6000 --grpc-port 10000 --grpc-ip node.testnet.concordium.com

# Transfer to another account

concordium-client contract update YOUR-CONTRACT-INSTANCE --entrypoint transfer --parameter-json cis2-fractionalizer/transfer-account.json --schema dist/schema.bin --sender --grpc-port 10000 --grpc-ip node.testnet.concordium.com --energy 6000 --grpc-port 10000 --grpc-ip node.testnet.concordium.com
