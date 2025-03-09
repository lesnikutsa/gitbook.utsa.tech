---
description: Passive diversification for the new token economy
---

# Entrypoint

## Links

* **Web**: [https://entrypoint.zone/](https://entrypoint.zone/)
* **Discord**: [https://discord.gg/gCE5NYPm7G](https://discord.gg/gCE5NYPm7G)
* **Github**: [https://github.com/entrypoint-zone/testnets/tree/main](https://github.com/entrypoint-zone/testnets/tree/main)

## **Details**

* **Network Chain ID**: entrypoint-pubtest-2
* **Binary**: entrypointd
* **Denom**: uentry
* **Working directory**: .entrypoint

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/ngyL41zQdXu](https://teletype.in/@lesnik13utsa/ngyL41zQdXu)
* **RPC**: [https://t-entrypoint.rpc.utsa.tech](https://t-entrypoint.rpc.utsa.tech)
* **API**: [https://t-entrypoint.api.utsa.tech](https://t-entrypoint.api.utsa.tech)
* **Explorer**: [https://exp.utsa.tech/entrypoint-test/staking](https://exp.utsa.tech/entrypoint-test/staking)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="a1583f1ba0f0f8b91bd163110b0bfd709604b266@65.108.206.118:61256"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.entrypoint/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.entrypoint/config/addrbook.json "https://share101.utsa.tech/entrypoint/addrbook.json"
```

