---
description: >-
  Nibiru is a sovereign proof-of-stake blockchain, open-source platform, and
  member of a family of interconnected blockchains that comprise the Cosmos
  Ecosystem
---

# Nibiru

## Links

* **Web**: [https://nibiru.fi/](https://nibiru.fi/)
* **Discord**: [https://discord.gg/KQShHXghT6](https://discord.gg/KQShHXghT6)
* **Github**: [https://github.com/NibiruChain/nibiru](https://github.com/NibiruChain/nibiru)

## **Details**

* **Network Chain ID**: cataclysm-1
* **Binary**: nibid
* **Denom**: unibi
* **Working directory**: .nibid

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/hqE5JY9G2fn](https://teletype.in/@lesnik13utsa/hqE5JY9G2fn)
* **RPC**: [https://m-nibiru.rpc.utsa.tech/ ](https://m-nibiru.rpc.utsa.tech/)
* **API**: [https://m-nibiru.api.utsa.tech/](https://m-nibiru.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/nibiru](https://exp.utsa.tech/nibiru)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="854cd20142ac86c919577914930a95d4751865c4@144.76.29.90:60656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.nibid/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.nibid/config/addrbook.json "https://share.utsa.tech/nibiru/addrbook.json"
```

