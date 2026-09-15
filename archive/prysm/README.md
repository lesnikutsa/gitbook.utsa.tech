# PRYSM



## Links

* **Web**:&#x20;
* **Discord**: [https://discord.gg/prysmnetwork](https://discord.gg/prysmnetwork)
* **Github**: [https://github.com/kleomedes/prysm](https://github.com/kleomedes/prysm)

## **Details**

* **Network Chain ID**: prysm-devnet-1
* **Binary**: prysmd
* **Denom**: uprysm
* **Working directory**: .prysm

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/Ol\_ULENy7v0](https://teletype.in/@lesnik13utsa/Ol_ULENy7v0)
* **RPC**: [https://t-prysm.rpc.utsa.tech/](https://t-prysm.rpc.utsa.tech/)
* **API**: [https://t-prysm.api.utsa.tech/](https://t-prysm.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/prysm-test/staking](https://exp.utsa.tech/prysm-test/staking)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="88ad3a3b9b981f0bbb52d5c996d0f7e1aa9426fa@65.108.206.118:61256"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.prysm/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.prysm/config/addrbook.json "https://share101.utsa.tech/prysm/addrbook.json"
```

