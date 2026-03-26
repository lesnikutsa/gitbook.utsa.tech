---
description: >-
  La Tanda is a blockchain project that digitalizes the classic savings circle
  model (tanda/ROSCA): participants form groups and take turns sharing a pool of
  funds
---

# La Tanda



## Links

* **Web**: [https://n8n.latanda.online/chain/](https://n8n.latanda.online/chain/)
* **Discord**: [https://discord.gg/Ve9M2ZSYC2](https://discord.gg/Ve9M2ZSYC2)
* **Github**: [https://github.com/INDIGOAZUL?tab=repositories](https://github.com/INDIGOAZUL?tab=repositories)

## **Details**

* **Network Chain ID**: latanda-testnet-1
* **Binary**: latandad
* **Denom**: ultd
* **Working directory**: .latanda

## Public services

* **Guide (RU)**:&#x20;
* **RPC**: [https://t-latanda.rpc.utsa.tech/](https://t-latanda.rpc.utsa.tech/)
* **API**: [https://t-latanda.api.utsa.tech/](https://t-latanda.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/latanda/staking](https://exp.utsa.tech/latanda/staking)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="d6217d85d3747ebbfbf75898bd4407da567f5291@65.108.206.118:60556"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.latanda/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.latanda/config/addrbook.json "https://share101.utsa.tech/latanda/addrbook.json"
```

