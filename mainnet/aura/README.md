---
description: Building the Internet of NFTs
---

# Aura

## Links

* **Web**: [https://aura.network/](https://aura.network/)
* **Discord**: [https://discord.gg/JscC9a8A83](https://discord.gg/JscC9a8A83)
* **Github**: [https://github.com/aura-nw/](https://github.com/aura-nw/testnets)

## **Details**

* **Network Chain ID**: aura\_6322-2
* **Binary**: aurad
* **Denom**: uaura
* **Working directory**: .aura

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/Bfy\_7vVq3wT](https://teletype.in/@lesnik13utsa/Bfy_7vVq3wT)
* **RPC**: [https://m-aura.rpc.utsa.tech/](https://m-aura.rpc.utsa.tech/)
* **API**: [https://m-aura.api.utsa.tech/](https://m-aura.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/aura/staking](https://exp.utsa.tech/aura/staking)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="f686e543584e7994d3c3402a5e80cbe36705a4cb@144.76.29.90:60756"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.aura/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.aura/config/addrbook.json "https://share.utsa.tech/aura/addrbook.json"
```

