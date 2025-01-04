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

* **Network Chain ID**: nibiru-itn-3
* **Binary**: nibid
* **Denom**: unibi unusd
* **Working directory**: .nibid

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/hqE5JY9G2fn](https://teletype.in/@lesnik13utsa/hqE5JY9G2fn)
* **RPC**: [https://t-nibiru.rpc.utsa.tech/](https://t-nibiru.rpc.utsa.tech/)
* **API**: [https://t-nibiru.api.utsa.tech/](https://t-nibiru.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/nibiru-test](https://exp.utsa.tech/nibiru-test/staking)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="2ea12c6e51eec2bac63c3d00d265c64bb07795ac@65.108.206.118:60656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.nibid/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.nibid/config/addrbook.json "https://share101.utsa.tech/nibiru/addrbook.json"
```

