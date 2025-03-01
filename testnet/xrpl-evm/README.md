# XRPL EVM

## Links

* **Web**:&#x20;
* **Discord**: [https://discord.gg/S26j3f5aqb](https://discord.gg/S26j3f5aqb)
* **Github**: [https://github.com/xrplevm/node](https://github.com/xrplevm/node)

## **Details**

* **Network Chain ID**: [https://teletype.in/@lesnik13utsa/r9Z\_bQwRqdd](https://teletype.in/@lesnik13utsa/r9Z_bQwRqdd)
* **Binary**: exrpd
* **Denom**: axrp
* **Working directory**: axrp

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/r9Z\_bQwRqdd](https://teletype.in/@lesnik13utsa/r9Z_bQwRqdd)
* **RPC**: [https://d-xrpl.rpc.utsa.tech/](https://d-xrpl.rpc.utsa.tech/)
* **API**: [https://d-xrpl.api.utsa.tech/](https://d-xrpl.api.utsa.tech/)
* **gRPC:** `d-xrpl.grpc.utsa.tech:433`
* **EVM RPC:** [https://d-xrpl.evm.utsa.tech/](https://d-xrpl.evm.utsa.tech/)
* **EVM WSS:** `wss://d-xrpl.wss.utsa.tech:443`
* **Explorer**: [https://exp.utsa.tech/xrpl-devnet/staking](https://exp.utsa.tech/xrpl-devnet/staking)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="196748bd1e1a3b2e2e2f22b54588d1545be0ecf9@5.9.87.231:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.exrpd/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.exrpd/config/addrbook.json "https://share102.utsa.tech/xrpl/addrbook.json"
```

