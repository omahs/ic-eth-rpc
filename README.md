# IC 🔗 ETH (Canister)

> #### Interact with the [Ethereum](https://ethereum.org/) blockchain from the [Internet Computer](https://internetcomputer.org/).

### Test canister: [6yxaq-riaaa-aaaap-abkpa-cai](https://a4gq6-oaaaa-aaaab-qaa4q-cai.raw.ic0.app/?id=6yxaq-riaaa-aaaap-abkpa-cai)

## Overview

**IC 🔗 ETH** is an Internet Computer canister smart contract for communicating with the Ethereum blockchain using an [on-chain API](./API.md). 

This canister facilitates API requests to Ethereum  services such as [Infura](https://www.infura.io/), [Gateway.fm](https://gateway.fm/), or [CloudFlare](https://www.cloudflare.com/en-gb/web3/) using [HTTPS outcalls](https://internetcomputer.org/docs/current/developer-docs/integrations/http_requests/), enabling functionality similar to traditional Ethereum dApps, including querying Ethereum smart contract states and submitting raw transactions.

Beyond the Ethereum blockchain, this canister also supports Polygon, Avalanche, and other popular EVM networks. Check out [this webpage](https://chainlist.org/) for a list of all supported networks and RPC providers.

## Quick Start

Add the following to your `dfx.json` config file:

```json
{
  "canisters": {
    "ic_eth": {
      "type": "custom",
      "candid": "https://github.com/internet-computer-protocol/ic-eth-rpc/releases/latest/download/ic_eth.did",
      "wasm": "https://github.com/internet-computer-protocol/ic-eth-rpc/releases/latest/download/ic_eth_dev.wasm.gz",
      "remote": {
        "id": {
          "ic": "6yxaq-riaaa-aaaap-abkpa-cai"
        }
      },
      "frontend": {}
    }
  }
}
```

Run the following commands to run the canister in your local environment:

```sh
# Start the local replica
dfx start --background

# Deploy the `ic_eth` canister
dfx deploy ic_eth

# Call the `eth_gasPrice` JSON-RPC method
dfx canister call ic_eth request '("https://cloudflare-eth.com/v1/mainnet", "{\"jsonrpc\":\"2.0\",\"method\":\"eth_gasPrice\",\"params\":[],\"id\":1}", 1000)' --wallet $(dfx identity get-wallet) --with-cycles 600000000
```

## Learn More

* [How this canister works behind the scenes](https://github.com/internet-computer-protocol/ic-eth-rpc/blob/main/DeepDive.md)
* [Canister API documentation](https://github.com/internet-computer-protocol/ic-eth-rpc/blob/main/API.md)

## Contributing

Contributions are welcome! Please check out the [contributor guidelines](https://github.com/internet-computer-protocol/ic-eth-rpc/blob/main/.github/CONTRIBUTING.md) for more information.

Run the following commands to set up a local development environment:

```bash
# Clone the repository
git clone https://github.com/internet-computer-protocol/ic-eth-rpc
cd ic-eth-rpc

# Deploy to the local replica
dfx start --background
dfx deploy
```

## Examples

### Ethereum RPC (local replica)
```bash
# Use a custom provider
dfx canister call ic_eth --wallet $(dfx identity get-wallet) --with-cycles 600000000 request '("https://cloudflare-eth.com","{\"jsonrpc\":\"2.0\",\"method\":\"eth_gasPrice\",\"params\":[],\"id\":1}",1000)'
dfx canister call ic_eth --wallet $(dfx identity get-wallet) --with-cycles 600000000 request '("https://ethereum.publicnode.com","{\"jsonrpc\":\"2.0\",\"method\":\"eth_gasPrice\",\"params\":[],\"id\":1}",1000)'

# Register your own provider
dfx canister call ic_eth register_provider '(record { chain_id=1; base_url="https://cloudflare-eth.com"; credential_path="/v1/mainnet"; cycles_per_call=10; cycles_per_message_byte=1; })'
dfx canister call ic_eth --wallet $(dfx identity get-wallet) --with-cycles 600000000 provider_request '(0,"{\"jsonrpc\":\"2.0\",\"method\":\"eth_gasPrice\",\"params\":[],\"id\":1}",1000)'
```

### Ethereum RPC (IC mainnet)
```bash
dfx canister --network ic call ic_eth --wallet $(dfx identity --network ic get-wallet) --with-cycles 600000000 request '("https://cloudflare-eth.com","{\"jsonrpc\":\"2.0\",\"method\":\"eth_gasPrice\",\"params\":[],\"id\":1}",1000)'
dfx canister --network ic call ic_eth --wallet $(dfx identity --network ic get-wallet) --with-cycles 600000000 request '("https://ethereum.publicnode.com","{\"jsonrpc\":\"2.0\",\"method\":\"eth_gasPrice\",\"params\":[],\"id\":1}",1000)'
```

### Authorization (local replica)

```bash
PRINCIPAL=$(dfx identity get-principal)
dfx canister call ic_eth authorize "(principal \"$PRINCIPAL\", variant { RegisterProvider })"
dfx canister call ic_eth get_authorized '(variant { RegisterProvider })'
dfx canister call ic_eth deauthorize "(principal \"$PRINCIPAL\", variant { RegisterProvider })"
```

## Related Projects

* [IC 🔗 ETH](https://github.com/dfinity/ic-eth-starter): a full-stack starter project for calling Ethereum smart contracts from an IC dapp.
* [Bitcoin canister](https://github.com/dfinity/bitcoin-canister): interact with the Bitcoin blockchain from the Internet Computer.