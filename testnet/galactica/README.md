# Galactica

## Links

* **Web**: [https://galactica.com/](https://galactica.com/)
* **Discord**: [https://discord.gg/galactica](https://discord.gg/galactica)
* **Github**: [https://github.com/Galactica-corp/networks/tree/main/galactica\_9302-1](https://github.com/Galactica-corp/networks/tree/main/galactica_9302-1)

## **Details**

* **Network Chain ID**: galactica\_9302-1
* **Binary**: galacticad
* **Denom**: agnet
* **Working directory**: .galactica
* **WASM:** NO

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/y3qN3FfEVT6](https://teletype.in/@lesnik13utsa/y3qN3FfEVT6)
* **RPC**: [https://t-galactica.rpc.utsa.tech/](https://t-galactica.rpc.utsa.tech/)
* **API**: [https://t-galactica.api.utsa.tech/](https://t-galactica.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/galactica-test](https://exp.utsa.tech/galactica-test)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="7175e583832c56d259a5b02098302798b0205dc4@65.108.206.118:60656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.galactica/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.galactica/config/addrbook.json "https://share101.utsa.tech/warden/addrbook.json"
```

