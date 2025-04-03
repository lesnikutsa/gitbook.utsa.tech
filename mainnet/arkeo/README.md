---
description: The Decentralized Back-End For All Node Data
---

# Arkeo

## Links

* **Web**: [https://arkeo.network/](https://arkeo.network/)
* **Discord**:&#x20;
* **Github**: [https://github.com/arkeonetwork/arkeo](https://github.com/arkeonetwork/arkeo)

## **Details**

* **Network Chain ID**: arkeo-main-v1
* **Binary**: arkeod
* **Denom**: uarkeo
* **Working directory**: .arkeo

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/pdaQdlKWbWS](https://teletype.in/@lesnik13utsa/pdaQdlKWbWS)
* **RPC**: [https://m-arkeo.rpc.utsa.tech/](https://m-arkeo.rpc.utsa.tech/)
* **API**: [https://m-arkeo.api.utsa.tech/](https://m-arkeo.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/arkeo-main/staking](https://exp.utsa.tech/arkeo-main/staking)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="092d3c2b64183c16be1b4be97dbafc9c270345c3@5.9.87.231:60556"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.arkeo/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.arkeo/config/addrbook.json "https://share102.utsa.tech/arkeo/addrbook.json"
```

