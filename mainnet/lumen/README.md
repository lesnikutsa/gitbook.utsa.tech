---
description: >-
  Lumen is a decentralized internet stack that integrates a browser, gateways,
  and blockchain
---

# Lumen

The native browser for the Lumen ecosystem provides direct access to blockchain state, IPFS content, and network gateways without centralized servers or trusted intermediaries

## Links

* **Web**:&#x20;
* **Discord**: [https://discord.gg/DwK6V9shKc](https://discord.gg/DwK6V9shKc)
* **Github**: [https://github.com/network-lumen/validator-kit/tree/master](https://github.com/network-lumen/validator-kit/tree/master)

## **Details**

* **Network Chain ID**: lumen
* **Binary**: lumend
* **Denom**: ulmn
* **Working directory**: .lumen

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/HYJHkYMK-K4](https://teletype.in/@lesnik13utsa/HYJHkYMK-K4)
* **RPC**:&#x20;
* **API**:&#x20;
* **Explorer**: [https://explorer.utsa.tech/networks/lumen-mainnet](https://explorer.utsa.tech/networks/lumen-mainnet)

## Peering

You can use peer UTSA for fast connection or state sync

```shell
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.lumen/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
#wget -O $HOME/.lumen/config/addrbook.json "https://share.utsa.tech/lumen/addrbook.json"
```

