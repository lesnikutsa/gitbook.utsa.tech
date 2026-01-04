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
* **RPC**: [https://m-lumen.rpc.utsa.tech/](https://m-lumen.rpc.utsa.tech/)
* **API**: [https://m-lumen.api.utsa.tech/](https://m-lumen.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/lumen/staking](https://exp.utsa.tech/lumen/staking)

## Peering

You can use peer UTSA for fast connection or state sync

```shell
peers="546d284c7b7f7a717b06d17002f28ee746ded36f@144.76.29.90:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.lumen/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.lumen/config/addrbook.json "https://share.utsa.tech/lumen/addrbook.json"
```

