---
description: >-
  LUXA Network is a sovereign Cosmos SDK Layer 1 secured by CometBFT
  proof-of-stake consensus. It supports native LUXA staking and governance,
  CosmWasm smart contracts, and IBC
---

# Luxa

## Links

* **Web**: [https://luxaecosystem.xyz/validator-guide](https://luxaecosystem.xyz/validator-guide)
* **TG**: [https://t.me/LuxaCommunity](https://t.me/LuxaCommunity)
* **Github**: [https://github.com/luxaecosystem/luxa-chain](https://github.com/luxaecosystem/luxa-chain)

## **Details**

* **Network Chain ID**: luxa-1
* **Binary**: luxad
* **Denom**: uluxa
* **Working directory**: .luxa

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/xtULT89ZKMs](https://teletype.in/@lesnik13utsa/xtULT89ZKMs)
* **RPC**: [https://t-luxa.rpc.utsa.tech/](https://t-luxa.rpc.utsa.tech/)
* **API**: [https://t-luxa.api.utsa.tech/](https://t-luxa.api.utsa.tech/)
* **Explorer UTSA**: [https://explorer.utsa.tech/networks/luxa-mainnet](https://explorer.utsa.tech/networks/luxa-mainnet)
* **Docs:** [https://luxaecosystem.xyz/validator-guide](https://luxaecosystem.xyz/validator-guide)

## Peering

You can use peer **UTSA** for fast connection or state sync

```shell
peers="4d27a8c6cfb9bdc53ce476c9216a4d76cabc8472@144.76.29.90:56656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.luxa/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.luxa/config/addrbook.json "https://share.utsa.tech/luxa/addrbook.json"
```

