---
description: Gitopia is the next-generation Code Collaboration Platform
---

# Gitopia



## Links

* **Web**: [https://gitopia.com/](https://gitopia.com/)
* **Discord**: [https://discord.gg/YAyb26JTPP](https://discord.gg/YAyb26JTPP)
* **Github**: [https://gitopia.com/home](https://gitopia.com/home)

## **Details**

* **Network Chain ID**: gitopia
* **Binary**: gitopiad
* **Denom**: ulore
* **Working directory**: .gitopia

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/K4wEQmEMj3W](https://teletype.in/@lesnik13utsa/K4wEQmEMj3W)
* **RPC**: [https://m-gitopia.rpc.utsa.tech/](https://m-gitopia.rpc.utsa.tech/)
* **API**: [https://m-gitopia.api.utsa.tech/](https://m-gitopia.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/gitopia/staking](https://exp.utsa.tech/gitopia/staking)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="6b9d7432f899e3bbafc3f0c11495b7f641f0f978@144.76.29.90:46656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.gitopia/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.gitopia/config/addrbook.json "https://share.utsa.tech/gitopia/addrbook.json"
```

