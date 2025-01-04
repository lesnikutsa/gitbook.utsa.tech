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

* **Network Chain ID**: mocha-4
* **Binary**: celestia-appd-test
* **Denom**: utia
* **Working directory**: .celestia-app-test

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/Pxyw1cdBaoZ#8ILi](https://teletype.in/@lesnik13utsa/Pxyw1cdBaoZ#8ILi)
* **RPC**: [https://t-celestia.archive.rpc.utsa.tech/](https://t-celestia.archive.rpc.utsa.tech/)
* **API**: [https://t-celestia.archive.api.utsa.tech/](https://t-celestia.archive.api.utsa.tech/)
* **gRPC:** t-celestia.archive.grpc.utsa.tech:443
* **Explorer**: [https://exp.utsa.tech/celestia-test/staking](https://exp.utsa.tech/celestia-test/staking)



## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="15eb02dd78fe034a9bb35ab325290004ddf67c9a@138.201.122.61:36656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.celestia-app-test/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.celestia-app-test/config/addrbook.json "https://share103.utsa.tech/celestia-testnet/addrbook.json"
```

