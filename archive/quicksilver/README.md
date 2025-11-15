---
description: QUICKSILVER THE COSMOS LIQUID STAKING ZONE
---

# Quicksilver

## Links

* **Web**: [https://quicksilver.zone/](https://quicksilver.zone/)
* **Discord**: [https://discord.gg/PKCMXYq9pf](https://discord.gg/PKCMXYq9pf)
* **Github**: [https://github.com/ingenuity-build/quicksilver/tags](https://github.com/ingenuity-build/quicksilver/tags)

## **Details**

* **Network Chain ID**: quicksilver-2
* **Binary**: quicksilverd
* **Denom**: uqck
* **Working directory**: .quicksilverd

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/pjA2UEqZiqV](https://teletype.in/@lesnik13utsa/pjA2UEqZiqV)
* **RPC**: [https://m-quicksilver.rpc.utsa.tech/](https://m-quicksilver.rpc.utsa.tech/)
* **API**: [https://m-quicksilver.api.utsa.tech/](https://m-quicksilver.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/quicksilver/staking](https://exp.utsa.tech/quicksilver/staking)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="78fc99f2f57edca0440a0afa83f36a01d56a0ea3@144.76.29.90:61156"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.quicksilverd/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.quicksilverd/config/addrbook.json "https://share.utsa.tech/quicksilver/addrbook.json"
```

