---
description: Dymension makes it easy to deploy your own blockchains called RollApps
---

# Dymension

The Dymension Hub is the calculated protocol layer and acts as a decentralized source of truth for the network. Dymension Hub is a Proof of Stake blockchain that provides consensus, security and liquidity for RollApps

## Links

* **Web**: [https://dymension.xyz](https://dymension.xyz/)
* **Discord**: [https://discord.gg/dymension](https://discord.gg/dymension)
* **Github**: [https://github.com/dymensionxyz](https://github.com/dymensionxyz/testnets/blob/main/dymension-hub/35-C/genesis_validators.md)

## **Details**

* **Network Chain ID**: dymension\_1100-1
* **Binary**: dymd
* **Denom**: adym
* **Working directory**: .dymension

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/cItIcj\_gkgF](https://teletype.in/@lesnik13utsa/cItIcj_gkgF)
* **RPC**: [https://m-dymension.rpc.utsa.tech/](https://m-dymension.rpc.utsa.tech/)
* **API**: [https://m-dymension.api.utsa.tech/](https://m-dymension.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/dymension](https://exp.utsa.tech/dymension)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="b0d5f8e7d2effd11f063bbd45e31cb3740da0039@144.76.29.90:61056"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.dymension/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.dymension/config/addrbook.json "https://share.utsa.tech/dymension/addrbook.json"
```

