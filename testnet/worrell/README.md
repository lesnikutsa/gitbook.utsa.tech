# Worrell

## Links

* **Web**: [https://worrellchain.com/#features](https://worrellchain.com/#features)
* **Discord**:&#x20;
* **Github**: [https://github.com/worrellchain/worrell](https://github.com/worrellchain/worrell)

## **Details**

* **Network Chain ID**: worrell-testnet-1
* **Binary**: worrelld
* **Denom**: uworrell
* **Working directory**: .worrell

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/5gAx-5RmEOD](https://teletype.in/@lesnik13utsa/5gAx-5RmEOD)
* **RPC**: [https://t-worrell.rpc.utsa.tech/](https://t-worrell.rpc.utsa.tech/)
* **API**: [https://t-worrell.api.utsa.tech/](https://t-worrell.api.utsa.tech/)
* **Explorer UTSA**: [https://explorer.utsa.tech/networks/worrell-testnet](https://explorer.utsa.tech/networks/worrell-testnet)
* **Docs:** [https://explorer.utsa.tech/networks/worrell-testnet](https://explorer.utsa.tech/networks/warrell-testnet)
* **Faucet:** `curl -X POST` [`http://164.68.98.186:4500`](http://164.68.98.186:4500) `\ -H "Content-Type: application/json" \ -d '{"address":"worrell1..."}'`



## Peering

You can use peer **UTSA** for fast connection or state sync

```shell
peers="0c9230beae58f3c1926ae6f61d8ecaafe1242a9b@65.21.136.112:56656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.worrell/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.worrell/config/addrbook.json "https://share103.utsa.tech/worrell/addrbook.json"
```

