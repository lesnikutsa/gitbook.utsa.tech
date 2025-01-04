# Dora Factory



## Links

* **Web**: [https://dorafactory.org/](https://dorafactory.org/)
* **Discord**: [https://discord.gg/7k3w77YNet](https://discord.gg/7k3w77YNet)
* **Github**: [https://github.com/DoraFactory/doravota](https://github.com/DoraFactory/doravota)

## **Details**

* **Network Chain ID**: vota-ash
* **Binary**: dorad
* **Denom**: peaka
* **Working directory**: .dora

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/ZT0Mq5hkUqQ](https://teletype.in/@lesnik13utsa/ZT0Mq5hkUqQ)
* **RPC**: [https://m-dora.rpc.utsa.tech/](https://m-dora.rpc.utsa.tech/)
* **API**: [https://m-dora.api.utsa.tech/](https://m-dora.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/dora](https://exp.utsa.tech/dora)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="0fcc76f46c1e438fd12cf9d4cb0ebcce1a5edd9b@144.76.29.90:60556"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.dora/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.dora/config/addrbook.json "https://share.utsa.tech/dora/addrbook.json"
```

