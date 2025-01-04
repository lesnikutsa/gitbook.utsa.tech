---
description: The Decentralized Back-End For All Node Data
---

# Arkeo

## Links

* **Web**: [https://arkeo.network/](https://arkeo.network/)
* **Discord**:&#x20;
* **Github**: [https://github.com/arkeonetwork/arkeo](https://github.com/arkeonetwork/arkeo)

## **Details**

* **Network Chain ID**: arkeo-testnet-3
* **Binary**: arkeod
* **Denom**: uarkeo
* **Working directory**: .arkeo

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/pdaQdlKWbWS](https://teletype.in/@lesnik13utsa/pdaQdlKWbWS)
* **RPC**: [https://t-arkeo.rpc.utsa.tech/](https://t-arkeo.rpc.utsa.tech/)
* **API**: [https://t-arkeo.api.utsa.tech/](https://t-arkeo.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/arkeo-test/staking](https://exp.utsa.tech/arkeo-test/staking)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="fd1f96034775faa95ce716dc419a548e65a5ae56@65.108.206.118:36656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.arkeo/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.arkeo/config/addrbook.json "https://share101.utsa.tech/arkeo/addrbook.json"
```

