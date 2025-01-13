---
description: >-
  AtomOne is a branch of Gaia that complements the broader Cosmos ecosystem with
  a security-conscious alternative hub. AtomOne recommits to the founding vision
  of the Cosmos Hub as a minimal IBC/ICS hub
---

# Atomone

## Links

* **Web**:&#x20;
* **Discord**: [https://discord.gg/fPxZwk3GS4](https://discord.gg/fPxZwk3GS4)
* **Github**: [https://github.com/atomone-hub](https://github.com/atomone-hub)

## **Details**

* **Network Chain ID**: atomone-1
* **Binary**: atomoned
* **Denom**: uatone
* **Working directory**: .atomone

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/yhwoF4hhHxP](https://teletype.in/@lesnik13utsa/yhwoF4hhHxP)
* **RPC**: [https://m-atomone.rpc.utsa.tech/](https://m-atomone.rpc.utsa.tech/)
* **API**: [https://m-atomone.api.utsa.tech/](https://m-atomone.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/atomone/staking](https://exp.utsa.tech/atomone/staking)
* **Restake**:&#x20;

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="0857c4b03734fb30b2fed2eabb8ba7b6788d6aae@5.9.87.231:61156"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.atomone/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.atomone/config/addrbook.json "https://share102.utsa.tech/atomone/addrbook.json"
```

