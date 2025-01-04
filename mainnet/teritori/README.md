---
description: >-
  Teritori is a multi-chain hub designed to allow IBC and non IBC communities to
  connect, trade services & NFTs, launch new projects & build further existing
  ones
---

# Teritori

## Links

* **Web**: [https://teritori.com/](https://teritori.com/)
* **Discord**: [https://discord.gg/dUh4ruqKf6](https://discord.gg/dUh4ruqKf6)
* **Github**: [https://github.com/TERITORI](https://github.com/TERITORI/teritori-chain/tree/teritori-testnet-v1)

## **Details**

* **Network Chain ID**: `teritori-1`
* **Binary**: `teritorid`
* **Denom**: `utori`
* **Working directory**: `.teritorid`

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/Qjmr1gGIjfd](https://teletype.in/@lesnik13utsa/Qjmr1gGIjfd)
* **RPC**: [https://m-teritori.rpc.utsa.tech/](https://m-teritori.rpc.utsa.tech/)
* **API**: [https://m-teritori.api.utsa.tech/](https://m-teritori.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/teritori/staking](https://exp.utsa.tech/teritori/staking)
* **Restake**: [https://restake.app/teritori/torivaloper1kunzrdg6u8gql4faj33lstghhqdtp59e0xgggy](https://restake.app/teritori/torivaloper1kunzrdg6u8gql4faj33lstghhqdtp59e0xgggy)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="d5f81fe3b6d5cc811d7460a69c020796cfaed438@144.76.29.90:36656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.teritorid/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.teritorid/config/addrbook.json "https://share.utsa.tech/teritori/addrbook.json"
```
