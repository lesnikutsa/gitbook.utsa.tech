---
description: The Business Grade Multi-Chain NFT Infrastructure for Web 3.0
---

# Uptick

## Links

* **Web**: [https://www.uptick.network/](https://www.uptick.network/)
* **Discord**: [https://discord.gg/r5XJCk6c4b](https://discord.gg/r5XJCk6c4b)
* **Github**: [https://github.com/UptickNetwork/uptick](https://github.com/UptickNetwork/uptick)

## **Details**

* **Network Chain ID**: uptick\_117-1
* **Binary**: uptickd
* **Denom**: auptick
* **Working directory**: .uptickd

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/q6eQOF4Xsd5](https://teletype.in/@lesnik13utsa/q6eQOF4Xsd5)
* **RPC**: [https://m-uptick.rpc.utsa.tech/](https://m-uptick.rpc.utsa.tech/)
* **API**: [https://m-uptick.api.utsa.tech/](https://m-uptick.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/uptick](https://exp.utsa.tech/uptick)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="e75bcaf0cf2d5dce359c1258121ecaa6d5b41991@144.76.29.90:60956"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.uptickd/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.uptickd/config/addrbook.json "https://share.utsa.tech/uptick/addrbook.json"
```

