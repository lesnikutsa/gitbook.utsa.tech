# Tellor

## Links

* **Web**: [https://tellor.io/](https://tellor.io/)
* **Discord**: [https://discord.gg/tellor](https://discord.gg/tellor)
* **Github**: [https://github.com/tellor-io/layer](https://github.com/tellor-io/layer)

## **Details**

* **Network Chain ID**: layertest-3
* **Binary**: layerd
* **Denom**: loya
* **Working directory**: .layer

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/7ibJohZtyhY](https://teletype.in/@lesnik13utsa/7ibJohZtyhY)
* **RPC**: [https://t-tellor.rpc.utsa.tech/](https://t-tellor.rpc.utsa.tech/)
* **API**: [https://t-tellor.api.utsa.tech/](https://t-tellor.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/tellor-test/staking](https://exp.utsa.tech/tellor-test/staking)



## Peering

You can use peer **UTSA** for fast connection or state sync

```shell
peers="3b15e1da275828c3fa52ab8ce1a0444bdddc958b@5.9.87.231:56656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.layer/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.layer/config/addrbook.json "https://share102.utsa.tech/tellor/addrbook.json"
```

