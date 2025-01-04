---
description: >-
  Jackal Protocol is the purpose-built blockchain for the everyday user. No more
  choosing between security or user-experience. With Jackal, protecting your
  valuable data has never been easier
---

# Jackal

## Links

* **Web**: [https://jackalprotocol.com/](https://jackalprotocol.com/)
* **Discord**: [https://discord.gg/CcqYcnnax5](https://discord.gg/CcqYcnnax5)
* **Github**: [https://github.com/JackalLabs/](https://github.com/JackalLabs/canine-mainnet-genesis/tree/main/instruc)

## **Details**

* **Network Chain ID**: jackal-1
* **Binary**: canined
* **Denom**: ujkl
* **Working directory**: .canine

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/rzIUMa9EKwg](https://teletype.in/@lesnik13utsa/rzIUMa9EKwg)
* **RPC**: [https://m-jackal.rpc.utsa.tech/](https://m-jackal.rpc.utsa.tech/)
* **API**: [https://m-jackal.api.utsa.tech/](https://m-jackal.api.utsa.tech/)
* **Explorer**: [https://exp.utsa.tech/jackal/staking](https://exp.utsa.tech/jackal/staking)
* **Restake**: [https://restake.app/jackal/jklvaloper1ejrn54x9wpxarmp7ux2a7mtgt4f8vp5xum9q4p](https://restake.app/jackal/jklvaloper1ejrn54x9wpxarmp7ux2a7mtgt4f8vp5xum9q4p)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="0912e9993b2070b82945399aa3cb34cb8c02fdc6@144.76.29.90:60856"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.canine/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.canine/config/addrbook.json "https://share.utsa.tech/jackal/addrbook.json"
```

