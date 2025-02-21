# 💻 ✔️config.yaml

Replace **\<!!!!!>** with your actual values

```bash
enabled: true
grpc_port: 29591
zrchain_rpc: "localhost:29557"
state_file: "$HOME/.zrchain/sidecar/cache.json"
operator_config: "$HOME/.zrchain/sidecar/eigen_operator_config.yaml"
network: "testnet"

eth_oracle:
  rpc:
    local: "http://127.0.0.1:29545"
    testnet: "<!!!!!>"  # Replace this endpoint with a valid ETH-Holskey RPC endpoint
    mainnet: "<!!!!!>"  # Replace this endpoint with a valid ETH-Mainnet RPC endpoint
  contract_addrs:
    service_manager: "0xa559CDb9e029fc4078170122eBf7A3e622a764E4"
    price_feeds:
      btc: "0xF4030086522a5bEEa4988F8cA5B36dbC97BeE88c"
      eth: "0x5f4eC3Df9cbd43714FE2740f5E3616155c5b8419"
    zenbtc:
      controller:
        testnet: "0xaCE3634AAd9bCC48ef6A194f360F7ACe51F7d9f1"
      token:
        ethereum:
          testnet: "0xfA32a2D7546f8C7c229F94E693422A786DaE5E18"
  network_name:
    mainnet: "Ethereum Mainnet"
    testnet: "Holešky Ethereum Testnet"

solana_rpc:
  testnet: "https://api.testnet.solana.com"
  mainnet: "<!!!!!>" # Replace this endpoint with a valid Solana mainnet RPC endpoint

proxy_rpc:
  url: # To be provided by the Zenrock team Can be ignored for Testnet
  user: # To be provided by the Zenrock team Can be ignored for Testnet
  password: # To be provided by the Zenrock team Can be ignored for Testnet

neutrino:
  path: "$HOME/.zrchain/neutrino"
```

