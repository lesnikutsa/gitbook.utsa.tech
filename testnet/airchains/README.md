---
description: zkRollups Made Easy For Everyone
---

# Airchains

## Links

* **Web**: [https://www.airchains.io/](https://www.airchains.io/)
* **Discord**: [https://discord.gg/airchains](https://discord.gg/airchains)
* **Github**: [https://github.com/airchains-network/junction](https://github.com/airchains-network/junction)

## **Details**

* **Network Chain ID**: varanasi-1
* **Binary**: junctiond
* **Denom**: umaf
* **Working directory**: .junctiond

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/vDwQ6cKmWKz](https://teletype.in/@lesnik13utsa/vDwQ6cKmWKz)
* **RPC**: [https://t-airchains.rpc.utsa.tech/](https://t-airchains.rpc.utsa.tech/)
* **API**: [https://t-airchains.api.utsa.tech/](https://t-airchains.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/airchains-test/staking](https://exp.utsa.tech/airchains-test/staking)

## Peering

You can use peer **UTSA** for fast connection or state sync

```shell
peers="38ffaf594a80b88ffaa0ecb3847bf0f77e5c52fe@5.9.87.231:36656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.junctiond/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.junctiond/config/addrbook.json "https://share102.utsa.tech/airchains/addrbook.json"
```

