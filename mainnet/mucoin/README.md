# MuCoin

## Links

* **Web**: [https://mucoin.org/staking](https://mucoin.org/staking)
* **Discord**: [https://discord.gg/VkeumyStM](https://discord.gg/VkeumyStM)
* **Github**: [https://github.com/dasgrid/mucoin](https://github.com/dasgrid/mucoin)

## **Details**

* **Network Chain ID**: mucoin-1
* **Binary**: mucoind
* **Denom**: umuc
* **Working directory**: .mucoin

## Public services

* **Guide:** [https://github.com/dasgrid/mucoin-node-deploy](https://github.com/dasgrid/mucoin-node-deploy)
* **Guide (RU)**:&#x20;
* **RPC**: [https://rpc.mucoin.org](https://rpc.mucoin.org)
* **API**: [https://rest.mucoin.org](https://rest.mucoin.org)
* **Explorer**: [https://explorer.vinjan-inc.com/mucoin/staking](https://explorer.vinjan-inc.com/mucoin/staking)

## Peering

You can use peer **UTSA** for fast connection or state sync

```shell
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.mucoin/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
#wget -O $HOME/.mucoin/config/addrbook.json "https://share.utsa.tech/mucoin/addrbook.json"
```

