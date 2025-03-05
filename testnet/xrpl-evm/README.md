# XRPL EVM

## Links

* **Web**:&#x20;
* **Discord**: [https://discord.gg/S26j3f5aqb](https://discord.gg/S26j3f5aqb)
* **Github**: [https://github.com/xrplevm/node](https://github.com/xrplevm/node)

## **Details**

* **Network Chain ID**: xrplevm\_1449000-1
* **Binary**: exrpd
* **Denom**: axrp
* **Working directory**: axrp

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/r9Z\_bQwRqdd](https://teletype.in/@lesnik13utsa/r9Z_bQwRqdd)
* **RPC**: xrplevm\_1449000-1
* **API**: [https://t-xrpl.api.utsa.tech/](https://t-xrpl.api.utsa.tech/)
* **gRPC:** t-xrpl.grpc.utsa.tech:433
* **EVM RPC:** t-xrpl.evm.utsa.tech
* **EVM WSS:** t-xrpl.evm.utsa.tech
* **Explorer**: [https://exp.utsa.tech/xrpl-testnet/staking](https://exp.utsa.tech/xrpl-testnet/staking)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="4a132daa35f22194e332cea80f5e25e7b28f2786@5.9.87.231:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.exrpd/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.exrpd/config/addrbook.json "https://share102.utsa.tech/xrpl/addrbook.json"
```

