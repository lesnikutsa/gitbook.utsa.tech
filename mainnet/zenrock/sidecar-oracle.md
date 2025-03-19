---
description: >-
  The validator sidecar service allows validators to vote on oracle data during
  the CometBFT consensus process
---

# 💻 Sidecar (✔️Oracle)

**Install the sidecar**

```shell
# create the necessary directories
mkdir -p $HOME/.zrchain/sidecar/bin
mkdir -p $HOME/.zrchain/sidecar/keys
```

**Install the binary file**

```shell
wget -O $HOME/.zrchain/sidecar/bin/zenrock-sidecar https://github.com/Zenrock-Foundation/zrchain/releases/download/v5.16.22/validator_sidecar
chmod +x $HOME/.zrchain/sidecar/bin/zenrock-sidecar
```

**Set a password for sidecar wallets**

```shell
read -p "Enter password for the keys: " key_pass
```

**Copy zenrock-validators**

```bash
cd $HOME
git clone https://github.com/zenrocklabs/zenrock-validators
```



**Build BLS binary**

```bash
cd $HOME/zenrock-validators/utils/keygen/bls/
go mod tidy
go build
```

**Create a BLS key**

```bash
bls_output_file=$HOME/.zrchain/sidecar/keys/bls.key.json
$HOME/zenrock-validators/utils/keygen/bls/bls --password $key_pass -output-file $bls_output_file
```

**Build ecdsa binary**

```bash
cd $HOME/zenrock-validators/utils/keygen/ecdsa/
go mod tidy
go build
```

**Create a ecdsa key**

```bash
ecdsa_output_file=$HOME/.zrchain/sidecar/keys/ecdsa.key.json
ecdsa_creation=$($HOME/zenrock-validators/utils/keygen/ecdsa/ecdsa --password $key_pass -output-file $ecdsa_output_file)
ecdsa_address=$(echo "$ecdsa_creation" | grep "Public address" | cut -d: -f2)
```

```shell
echo "ecdsa address: $ecdsa_address"
#Public address:  0xe766944B5a25B1DE938eF19D15D1F5d3B4fe6E2D
```

{% hint style="warning" %}
**IMPORTANT** - to continue you need to register on https://app.infura.io and get the following endpoints:&#x20;

* mainnet eth: [https://xxx](https://xxx)
* mainnet eth: wss://xxx
* holesky eth: [https://xxx](https://xxx)
{% endhint %}

**Create a configuration file config.yaml**

Replace the following variables with your values:

* <[https://rpc-endpoint-holesky-here](https://rpc-endpoint-holesky-here)>
* <[https://rpc-endpoint-mainnet-here](https://rpc-endpoint-mainnet-here)>
* zrchain\_rpc: ''localhost:HERE\_IS\_YOUR\_GRPC\_PORT''

```shell
nano $HOME/.zrchain/sidecar/config.yaml
```

```bash
enabled: true
grpc_port: 9191
zrchain_rpc: "localhost:9190"
state_file: "cache.json"
operator_config: "/root/.zrchain/sidecar/eigen_operator_config.yaml"
network: "mainnet"
eth_oracle:
  rpc:
    local: "http://127.0.0.1:8545"
    testnet: "<https://rpc-endpoint-holesky-here> "
    mainnet: "<https://rpc-endpoint-mainnet-here>"
  contract_addrs:
    service_manager: "0x4ca852BD78D9B7295874A7D223023Bff011b7EB3"
    price_feeds:
      btc: "0xF4030086522a5bEEa4988F8cA5B36dbC97BeE88c"
      eth: "0x5f4eC3Df9cbd43714FE2740f5E3616155c5b8419"
    zenbtc:
      controller:
        mainnet: "0xa87bE298115bE701A12F34F9B4585586dF052008"
      token:
        ethereum:
          mainnet: "0x2fE9754d5D28bac0ea8971C0Ca59428b8644C776"
  network_name:
    mainnet: "Ethereum Mainnet"
    testnet: "Holešky Ethereum Testnet"
solana_rpc:
  testnet: "https://api.testnet.solana.com"
  mainnet: "https://api.mainnet-beta.solana.com/"
proxy_rpc:
  url:
  user:
  password:
neutrino:
  path: "/root/.zrchain/sidecar/root-data/neutrino"
```

**Create a configuration file eigen\_operator\_config.yaml**

Replace the following variables with your values:

* \<ETH MAIN ENDPOINT HERE>
* \<ETH MAIN WSS ENDPOINT HERE>
* \<VALUE FROM STEP - ECDSA key>
* \<VALUE FROM STEP - zenvaloper address>

```bash
nano $HOME/.zrchain/sidecar/eigen_operator_config.yaml
```

```bash
register_operator_on_startup: true
register_on_startup: true
production: true

ecdsa_private_key_store_path: /root/.zrchain/sidecar/keys/ecdsa.key.json
bls_private_key_store_path: /root/.zrchain/sidecar/keys/bls.key.json

aggregator_server_ip_port_address: avs-aggregator.diamond.zenrocklabs.io:8090

eth_rpc_url: <ETH MAIN ENDPOINT HERE>
eth_ws_url: <ETH MAIN WSS ENDPOINT HERE>

enable_metrics: true
eigen_metrics_ip_port_address: 0.0.0.0:9292
enable_node_api: true
node_api_ip_port_address: 0.0.0.0:9191

operator_address: <VALUE FROM STEP - ECDSA key>
operator_validator_address: <VALUE FROM STEP - zenvaloper address>

avs_registry_coordinator_address: 0xFbFECE8f29f499c32206d8bFfA57da2b124790C7
operator_state_retriever_address: 0x03d0452e70711f169eB6B6F5Ab33d8571c313ef6

token_strategy_addr: 0xa5430Ca83713F877B77b54d5A24FD3D230DF854B
```

**Create a service file**

```bash
tee /etc/systemd/system/zenrock-sidecar.service > /dev/null <<EOF
[Unit]
Description=Zenrock-sidecar
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/.zrchain/sidecar/bin/zenrock-sidecar
Restart=on-failure
RestartSec=30
LimitNOFILE=65535
Environment="OPERATOR_BLS_KEY_PASSWORD=$key_pass"
Environment="OPERATOR_ECDSA_KEY_PASSWORD=$key_pass"
Environment="SIDECAR_CONFIG_FILE=$HOME/.zrchain/sidecar/config.yaml"

[Install]
WantedBy=multi-user.target
EOF
```

```bash
systemctl daemon-reload
systemctl enable zenrock-sidecar
systemctl restart zenrock-sidecar && journalctl -u zenrock-sidecar -f -o cat
```

{% hint style="warning" %}
**Don't forget to save the directory $HOME/.zrchain/sidecar/**
{% endhint %}

