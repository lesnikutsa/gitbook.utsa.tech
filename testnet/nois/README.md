---
description: >-
  Brings random beacons to Cosmos blockchains without compromising security or
  usability by leveraging drand and IBC
---

# Nois



## Links

* **Web**: [https://nois.network/](https://nois.network/)
* **Discord**: [https://discord.com/invite/SMp49GF6fF](https://discord.com/invite/SMp49GF6fF)
* **Github**: [https://github.com/noislabs](https://github.com/noislabs)

## **Details**

* **Network Chain ID**: nois-testnet-005
* **Binary**: noisd
* **Denom**: unois
* **Working directory**: .noisd

## Public services

* **Guide (RU)**:&#x20;
* **RPC**:&#x20;
* **API**:&#x20;
* **Explorer**: [https://explorer.stavr.tech/nois/staking](https://explorer.stavr.tech/nois/staking)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.noisd/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.noisd/config/addrbook.json ""
```

