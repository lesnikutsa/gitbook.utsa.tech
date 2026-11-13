---
description: >-
  Limonata is an EVM-compatible Layer 1 built with the Cosmos SDK. It provides
  smart contract execution and protocol-level gas sponsorship, with LIMO used
  for gas and staking
---

# Limonata



## Links

* **Web**: [https://limonata.xyz/](https://limonata.xyz/)
* **Discord**: [https://discord.gg/PauX77usMS](https://discord.gg/PauX77usMS)
* **Github**: [https://github.com/Limonata-Blockchain/limonata](https://github.com/Limonata-Blockchain/limonata)

## **Details**

* **Network Chain ID**: limonata\_20777-1
* **Binary**: limonatad
* **Denom**: aLIMO
* **Working directory**: .limonata-mainnet

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/RD3eEH50Zdv](https://teletype.in/@lesnik13utsa/RD3eEH50Zdv)
* **RPC**:&#x20;
* **API**:&#x20;
* **Explorer**: [https://explorer.utsa.tech/networks/limonata-mainnet](https://explorer.utsa.tech/networks/limonata-mainnet)

## Peering

You can use peer **UTSA** for fast connection or state sync

```shell
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.limonata-mainnet/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.limonata-mainnet/config/addrbook.json "https://share101.utsa.tech/limonata/addrbook.json"
```

