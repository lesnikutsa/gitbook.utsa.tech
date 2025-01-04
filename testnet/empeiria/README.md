---
description: The first End-to-End Verifiable Data Infrastructure (EVDI)
---

# Empeiria

## Links

* **Web**: [https://empe.io/](https://empe.io/)
* **Telegram:** [**https://t.me/EmpeValidators**](https://t.me/EmpeValidators)
* **Github**: [https://github.com/empe-io/empe-chains](https://github.com/empe-io/empe-chains)

## **Details**

* **Network Chain ID**: empe-testnet-2
* **Binary**: emped
* **Denom**: uempe
* **Working directory**: .empe-chain
* **WASM:** NO

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/B2TYTCTMisa](https://teletype.in/@lesnik13utsa/B2TYTCTMisa)
* **RPC**: [https://t-empeiria.rpc.utsa.tech/](https://t-empeiria.rpc.utsa.tech/)
* **API**: [https://t-empeiria.api.utsa.tech/](https://t-empeiria.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/empeiria-test/staking](https://exp.utsa.tech/empeiria-test/staking)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="1efd046506c3534bc34dbb410801a63b58d8aebd@5.9.87.231:46656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.empe-chain/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.empe-chain/config/addrbook.json "https://share102.utsa.tech/empeiria/addrbook.json"
```

