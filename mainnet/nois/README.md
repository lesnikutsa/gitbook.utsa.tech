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

* **Network Chain ID**: nois-1
* **Binary**: noisd
* **Denom**: unois
* **Working directory**: .noisd

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/d8TiWacZewd](https://teletype.in/@lesnik13utsa/d8TiWacZewd)
* **RPC**: [https://m-nois.rpc.utsa.tech/](https://m-nois.rpc.utsa.tech/)
* **API**: [https://m-nois.api.utsa.tech/](https://m-nois.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/nois](https://exp.utsa.tech/nois)
* **Restake**: [https://restake.app/nois/noisvaloper10hmy2yhwhqqe8p7ax0hrdt4uxkumlhcf7tkkk2](https://restake.app/nois/noisvaloper10hmy2yhwhqqe8p7ax0hrdt4uxkumlhcf7tkkk2)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="9b873fbb5afeb97eb1fb2ed5cbac1142445e161f@144.76.29.90:61456"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.noisd/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.noisd/config/addrbook.json "https://share.utsa.tech/nois/addrbook.json"
```

