# Konstellation

## Links

* **Web**: [https://konstellation.tech/](https://konstellation.tech/)
* **Discord**:&#x20;
* **Github**: [https://github.com/konstellation/konstellation](https://github.com/konstellation/konstellation)

## **Details**

* **Network Chain ID**: darchub
* **Binary**: knstld
* **Denom**: udarc
* **Working directory**: .knstld

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/fSCdiJ2oINK](https://teletype.in/@lesnik13utsa/fSCdiJ2oINK)
* **RPC**: [https://m-konstellation.rpc.utsa.tech/](https://m-konstellation.rpc.utsa.tech/)
* **API**: [https://m-konstellation.api.utsa.tech/](https://m-konstellation.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/konstellation/staking](https://exp.utsa.tech/konstellation/staking)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="e5e1f21957a07f623fb310737205b5b6cb1fce42@8.52.201.252:60756"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.knstld/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.knstld/config/addrbook.json "https://share106-7.utsa.tech/konstellation/addrbook.json"
```

