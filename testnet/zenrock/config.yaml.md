# 💻 ✔️config.yaml

Replace **\<!!!!!>** with your actual values

```bash
enabled: true
grpc_port: 9191
state_file: "cache.json"
zrchain_rpc: "localhost:9090"
operator_config: "eigen_operator_config.yaml"
network: "testnet"
eth_rpc:
  local: "http://127.0.0.1:8545"
  testnet: "https://rpc-endpoint-holesky-here"  # Replace this endpoint with a valid one
  mainnet: "https://rpc-endpoint-mainnet-here"  # Replace this endpoint with a valid one
solana_rpc:
  testnet: "https://api.testnet.solana.com"
  mainnet: "https://api.mainnet-beta.solana.com/"
neutrino:
  path: "./neutrino"
#for the proxy_rpc use the same credentials that were supplied as before
proxy_rpc:
  url: 
  user:
  password: 
```

