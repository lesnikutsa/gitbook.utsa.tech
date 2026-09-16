# Nexarail

## Links

* **Web**:&#x20;
* **Discord**: [https://discord.gg/Vda9T6J7eu](https://discord.gg/Vda9T6J7eu)
* **Github**: [https://github.com/Bookings-cpu/nexarail](https://github.com/Bookings-cpu/nexarail)

## **Details**

* **Network Chain ID**: nexarail-mainnet-2
* **Binary**: nexaraild
* **Denom**: unxrl
* **Working directory**: .nexarail

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/TUxVv0fAl51](https://teletype.in/@lesnik13utsa/TUxVv0fAl51)
* **RPC**: [https://m-nexarail.rpc.utsa.tech/](https://m-nexarail.rpc.utsa.tech/)
* **API**: [https://m-nexarail.api.utsa.tech/](https://m-nexarail.api.utsa.tech/)
* **Explorer**: [https://explorer.utsa.tech/networks/nexarail-mainnet](https://explorer.utsa.tech/networks/nexarail-mainnet)

## Peering

You can use peer **UTSA** for fast connection or state sync

```shell
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.nexarail/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
#wget -O $HOME/.nexarail/config/addrbook.json "https://share.utsa.tech/nexarail/addrbook.json"
```

