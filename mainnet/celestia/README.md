---
description: The first modular blockchain network
---

# Celestia

## Links

* **Web**: [https://celestia.org/](https://celestia.org/)
* **Discord**: [https://discord.gg/Y4fqeE8TCq](https://discord.gg/Y4fqeE8TCq)
* **Github**: [https://github.com/celestiaorg/](https://github.com/celestiaorg/)
* **Docs:** [https://docs.celestia.org/](https://docs.celestia.org/)

## **Details**

* **Network Chain ID**: celestia
* **Binary**: celestia-appd
* **Denom**: utia
* **Working directory**: .celestia-app-main

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/5Az1NXvr9Af](https://teletype.in/@lesnik13utsa/5Az1NXvr9Af)
* **RPC**: [https://m-celestia.archive.rpc.utsa.tech/](https://m-celestia.archive.rpc.utsa.tech/)
* **API**: [https://m-celestia.archive.api.utsa.tech/](https://m-celestia.archive.api.utsa.tech/)
* **gRPC:** m-celestia.archive.grpc.utsa.tech:443
* **Explorer**: [https://exp.utsa.tech/celestia-main/](https://exp.utsa.tech/celestia-main/)



## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="9d80179e85443b78627caac6c31bdd16ef2776cf@148.251.90.250:46656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.celestia-app-main/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.celestia-app-main/config/addrbook.json "https://share106.00.utsa.tech/celestia-mainnet/addrbook.json"
```

