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

* **Network Chain ID**: froopyland\_100-1
* **Binary**: dymd
* **Denom**: udym
* **Working directory**: .dymension

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/cItIcj\_gkgF](https://teletype.in/@lesnik13utsa/cItIcj_gkgF)
* **RPC**: [https://t-dymension.rpc.utsa.tech/](https://t-dymension.rpc.utsa.tech/)
* **API**: [https://t-dymension.api.utsa.tech/](https://t-dymension.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/dymension-test](https://exp.utsa.tech/dymension-test)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="05d4f45e4f935ea2ad44c29b64c22a9337fa3192@65.108.206.118:60756"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.dymension/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.dymension/config/addrbook.json "https://share101.utsa.tech/dymension/addrbook.json"
```

