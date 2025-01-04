---
description: >-
  Modern Layer 1 Cosmos SDK based blockchain with EVM, powered by the community.
  Made for Art and Science: bioinformatics, in silico research, AI, with NFTs,
  DAOs, Web3, VR, games and education in mind
---

# Genesis L1

## Links

* **Web**: [https://genesisl1.com/](https://genesisl1.com/)
* **Discord**: [https://discord.gg/KH2TtTxaEy](https://discord.gg/KH2TtTxaEy)
* **Github**: [https://github.com/alpha-omega-labs/genesisd](https://github.com/alpha-omega-labs/genesisd)

## **Details**

* **Network Chain ID**: genesis\_29-2
* **Binary**: genesisd
* **Denom**: el1
* **Working directory**: .genesisd

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/6lS-dR6v\_FY](https://teletype.in/@lesnik13utsa/6lS-dR6v_FY)
* **RPC**: [https://m-l1.rpc.utsa.tech/](https://m-l1.rpc.utsa.tech/)
* **API**: [https://m-l1.api.utsa.tech/](https://m-l1.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/genesis/staking](https://exp.utsa.tech/genesis/staking)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.genesisd/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.genesisd/config/addrbook.json "https://anode.team/GenesisL1/main/addrbook.json"
```
