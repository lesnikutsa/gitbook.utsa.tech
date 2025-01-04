---
description: >-
  Zenrock enables decentralized key management for builders of cross-chain
  protocols and wallets to unlock omnichain security.
---

# Zenrock

## Links

* **Web**: [https://www.zenrocklabs.io/](https://www.zenrocklabs.io/)
* **Discord**: [https://discord.gg/zenrockfoundation](https://discord.gg/zenrockfoundation)
* **Github**: [https://github.com/zenrocklabs/zenrock-validators](https://github.com/zenrocklabs/zenrock-validators)

## **Details**

* **Network Chain ID**: gardia-2
* **Binary**: `zenrockd` `zenrock-sidecar`
* **Denom**: urock
* **Working directory**: .zrchain

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/gLnp0FkzkOv](https://teletype.in/@lesnik13utsa/gLnp0FkzkOv)
* **RPC:** [https://t-zenrock.archive.rpc.utsa.tech/](https://t-zenrock.archive.rpc.utsa.tech/)
* **API:** [https://t-zenrock.archive.api.utsa.tech/](https://t-zenrock.archive.api.utsa.tech/)
* **EXP:** [https://exp.utsa.tech/zenrock-test/staking](https://exp.utsa.tech/zenrock-test/staking)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="400b7677af8494ea58d722a4f40c54de98b2ed22@8.52.201.252:36656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.zrchain/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.zrchain/config/addrbook.json "https://share106-7.utsa.tech/zenrock/addrbook.json"
```
