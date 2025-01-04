# Dora Factory



## Links

* **Web**: [https://dorafactory.org/](https://dorafactory.org/)
* **Discord**: [https://discord.gg/7k3w77YNet](https://discord.gg/7k3w77YNet)
* **Github**: [https://github.com/DoraFactory/doravota](https://github.com/DoraFactory/doravota)

## **Details**

* **Network Chain ID**: vota-testnet
* **Binary**: dorad
* **Denom**: peaka
* **Working directory**: .dora

## Public services

* **Guide (RU)**:&#x20;
* **RPC**:&#x20;
* **API**:&#x20;
* **Explorer**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.dora/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.dora/config/addrbook.json "https://testnet-files.itrocket.net/dora/addrbook.json"
```

