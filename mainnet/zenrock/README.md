---
description: >-
  Zenrock enables decentralized key management for builders of cross-chain
  protocols and wallets to unlock omnichain security.
---

# Zenrock

## Links

* **Web**: [https://www.zenrocklabs.io/](https://www.zenrocklabs.io/)
* **Discord**: [https://discord.gg/zenrockfoundation](https://discord.gg/zenrockfoundation)
* **Github**: [https://github.com/Zenrock-Foundation/zrchain/releases/](https://github.com/Zenrock-Foundation/zrchain/releases/)

## **Details**

* **Network Chain ID**: diamond-1
* **Binary**: `zenrockd` `zenrock-sidecar`
* **Denom**: urock
* **Working directory**: .zrchain

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/gLnp0FkzkOv](https://teletype.in/@lesnik13utsa/gLnp0FkzkOv)
* **RPC:** [https://m-zenrock.rpc.utsa.tech/](https://m-zenrock.rpc.utsa.tech/)
* **API:** [https://m-zenrock.api.utsa.tech/](https://m-zenrock.api.utsa.tech/)
* **EXP:** [https://exp.utsa.tech/zenrock/staking](https://exp.utsa.tech/zenrock/staking)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="48f518867d52a0d635436441ff8df4c1582caeac@8.52.201.252:36656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.zrchain/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.zrchain/config/addrbook.json "https://share.utsa.tech/zenrock/addrbook.json"
```
