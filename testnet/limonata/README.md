# Limonata



## Links

* **Web**:&#x20;
* **Discord**:&#x20;
* **Github**:&#x20;

## **Details**

* **Network Chain ID**: limonata\_10777-1
* **Binary**: limonatad
* **Denom**: aLIMO
* **Working directory**: .evmd

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/RD3eEH50Zdv](https://teletype.in/@lesnik13utsa/RD3eEH50Zdv)
* **RPC**: [https://t-limonata.rpc.utsa.tech/](https://t-limonata.rpc.utsa.tech/)
* **API**: [https://t-limonata.api.utsa.tech/](https://t-limonata.api.utsa.tech/)
* **Explorer**: [https://explorer.limonata.xyz/](https://explorer.limonata.xyz/)
* **Explorer**: [https://exp.utsa.tech/limonata-test/staking](https://exp.utsa.tech/limonata-test/staking)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="4bcab578e66bee865dbcf8fc046bab8f21546b13@65.108.206.118:60856"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.evmd/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.evmd/config/addrbook.json "https://share101.utsa.tech/limonata/addrbook.json"
```

