---
description: >-
  Bitcoin's Liquidity BLISS Stack (Bitcoin liquidity, Interoperability and
  Secured Scaling Stack) unites the Bitcoin ecosystem, unlocking direct access
  to BTC, Runes, Ordinals and BRC-20s, across multip
---

# Native

## Links

* **Web**: [https://www.gonative.cc/](https://www.gonative.cc/)
* **Discord**: [https://discord.gg/MNZ4wnN3XM](https://discord.gg/MNZ4wnN3XM)
* **Github**: [https://github.com/gonative-cc/gonative/releases](https://github.com/gonative-cc/gonative/releases)

## **Details**

* **Network Chain ID**: native-t1
* **Binary**: gonatived
* **Denom**: untiv
* **Working directory**: .gonative

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/Vj7SznlmE6n](https://teletype.in/@lesnik13utsa/Vj7SznlmE6n)
* **RPC**: [https://t-native.rpc.utsa.tech/](https://t-native.rpc.utsa.tech/)
* **API**: [https://t-native.api.utsa.tech/](https://t-native.api.utsa.tech/)
* **Explorer**: [https://testnet.native.explorers.guru/](https://testnet.native.explorers.guru/)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="f82c497af58874eac7c862c362df9659d95f5332@5.9.87.231:60756"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.gonative/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.gonative/config/addrbook.json "https://share102.utsa.tech/native/addrbook.json"
```

