---
hidden: true
---

# Warden Protocol

## Links

* **Web**: [https://wardenprotocol.org/#vision](https://wardenprotocol.org/#vision)
* **Discord**: [https://discord.gg/wardenprotocol](https://discord.gg/wardenprotocol)
* **Github**: [https://github.com/warden-protocol/wardenprotocol](https://github.com/warden-protocol/wardenprotocol)
* **Docs:** [https://docs.wardenprotocol.org/operate-a-node/barra-testnet/join-barra](https://docs.wardenprotocol.org/operate-a-node/barra-testnet/join-barra)

## **Details**

* **Network Chain ID**: warden\_8765-1
* **Binary**: wardend
* **Denom**: award
* **Working directory**: .warden

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/Pw2GAVJJ\_7y](https://teletype.in/@lesnik13utsa/Pw2GAVJJ_7y)
* **RPC**: [https://t-warden.rpc.utsa.tech/](https://t-warden.rpc.utsa.tech/)
* **API**: [https://t-warden.api.utsa.tech/](https://t-warden.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/warden/staking](https://exp.utsa.tech/warden/staking)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.warden/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.warden/config/addrbook.json "https://share.utsa.tech/warden/addrbook.json"
```

