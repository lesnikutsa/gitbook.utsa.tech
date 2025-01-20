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

* **Network Chain ID**: gardia-3
* **Binary**: `zenrockd` `zenrock-sidecar`
* **Denom**: urock
* **Working directory**: .zrchain

## Public services

* **Guide (RU)**:&#x20;
* **RPC:**&#x20;
* **API:**&#x20;
* **EXP:** [https://explorer.gardia.zenrocklabs.io/](https://explorer.gardia.zenrocklabs.io/)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
#peers=""
#sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.zrchain/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
#wget -O $HOME/.zrchain/config/addrbook.json "https://share106-7.utsa.tech/zenrock/addrbook.json"
```
