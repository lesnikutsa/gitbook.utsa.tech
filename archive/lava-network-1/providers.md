# ⌚ Providers

* Docs providers - [https://docs.lavanet.xyz/provider/](https://docs.lavanet.xyz/provider/)
* Dashboard - [https://info.lavanet.xyz/providers](https://info.lavanet.xyz/providers)
* Russian-language guide - [https://teletype.in/@lesnik13utsa/cnNf3QRB0xW](https://teletype.in/@lesnik13utsa/cnNf3QRB0xW)



{% hint style="warning" %}
**!!!IMPORTANT!!!**&#x20;

**In testnet2, you now need to configure TLS certificates for the provider - more details here - https://docs.lavanet.xyz/provider-tls/?utm\_source=lava-discord\&utm\_medium=discord\&utm\_campaign=testnet-fork**&#x20;
{% endhint %}

{% hint style="success" %}
Please note that this program is not incentive. The team encourages all potential providers to apply and participate in this program to help build a reliable and secure Lava network. To become a provider, please complete this form - [https://form.typeform.com/to/zB7JLVRs](https://form.typeform.com/to/zB7JLVRs)
{% endhint %}

{% hint style="warning" %}
A provider can get a **jail** status if during the last 3 epochs (1.5 hours) consumers complain about it and they have more than 95% errors in the first 10 messages or if the connection is lost In the current testnet, the slashing mechanism is disabled - therefore, providers should receive their test coins back in 25 hours. The team does not plan to introduce stricter restrictions - therefore, after the temporary jail stage, it will be possible to return to the work of the provider In the future, it is possible to enable slashing for getting into the jail, but this is not yet in the nearest plans
{% endhint %}

{% hint style="info" %}
When placing bets as a provider in transactions, four main parameters are used:

**Stake:** the amount of LAVA that will be sent to the stake. For a test network, this value should be at least 50000 LAVA per network. The value can be greater, but not less&#x20;

**Geolocation:** Specifies the geolocation of your server. US corresponds to 1 and EU corresponds to 2

**ChainID:** Identifier of the target blockchain network such as Cosmos Mainnet, Ethereum Ropsten, etc.

**Endpoints:** A list of endpoints, each specifying an address, geolocation, and an API like REST, JSON-RPC, etc. It is important to note that all provider requests are made using gRPC
{% endhint %}

{% hint style="info" %}
There are several ways to find out the ChainID - by going to the [dashboard](https://info.lavanet.xyz/providers) or using information from [RPC](https://public-rpc.lavanet.xyz/rest/lavanet/lava/spec/show_all_chains) or using the following command

````
lavad q spec list-spec --node https://public-rpc.lavanet.xyz:443/rpc/ | grep index```
````
{% endhint %}

Providers are the backbone of the Lava network, serving relay requests by staking on the network and managing RPC nodes on relay chains (eg Cosmos, Ethereum, Osmosis, Polygon, etc.). In return, providers are paid in the form of LAVA tokens for serving these requests

## Configuring the LAV1 provider

LAV1 is a chain identifier that allows you to become a provider using the LAVA testnet. This network is ideal for starting a provider. After you can easily add other networks In this guide, we will be using a staking validator wallet and a synchronized lava node, so it is assumed that we already have an up-to-date version of lava and other networks installed on our server. Our server is located in Europe, so we will use `--geolocation 2`

**0 - Download the lavap binary file**

{% hint style="info" %}
From 10/02/2023, a separate lavap binary file must be used for the provider. You can also configure lavavisor (analogous to cosmovisor). More details [here](https://discord.com/channels/963778337904427018/963781907152244796/1148592433421094932)
{% endhint %}

```bash
git clone https://github.com/lavanet/lava && cd lava
export LAVA_BINARY=lavap
make install
lavap version
#v1.0.4
```

**1 - Create or restore a wallet and replenish it with at least 50000LAVA**

For a testnet, you must use `--keyring-backend test` as it is necessary to sign transactions. It will be fixed in mainnet

```bash
# create wallet
lavad keys add <name_wallet> --keyring-backend test

# restore wallet (after insert seed command)
lavad keys add <name_wallet> --recover --keyring-backend test

# check balance
lavad q bank balances <address>
```

**2 - Registering a stake for the network we need. In this case LAV1 (lava-testnet-1)**

In the example below, we have specified port 12345. This port number is intended to inform others that your ISP's services will work at this address with this particular port number. Please don't confuse provider\_port with RPC, API or gRPC port numbers! You can change this port to the value you need, **but don't forget to make this port open to the outside world**. Also change \<name\_wallet>  \<server\_ip> \<moniker> to your own values

{% hint style="info" %}
**IMPORTANT - before you begin, you must configure a TLS certificate**
{% endhint %}

```bash
lavap tx pairing stake-provider "LAV1" \
    "50000000000ulava" \
    "<lava.your-site:443>,2,tendermintrpc,rest,grpc" 2 \
    --from "<name_wallet>" \
    --provider-moniker "<moniker>" \
    --keyring-backend "test" \
    --gas="auto" \
    --gas-adjustment "1.5" \
    --fees 5000ulava
```

{% hint style="success" %}
**Congratulations** - you have officially declared yourself as a service provider in LAVA! To register other networks, send another transaction replacing LAV1 with the desired value of another network! For example, the transaction for osmosis testnet would look like this:

lavap tx pairing stake-provider "COS4"\
"50000000000ulava"\
"[osmosis.your-site:443](https://osmosis.your-site),2,tendermintrpc,rest,grpc" 2\
\--from "\<name\_wallet>"\
\--provider-moniker ""\
\--keyring-backend "test"\
\--gas="auto"\
\--gas-adjustment "1.5"\
\--fees 5000ulava
{% endhint %}

It's time to check the status of your provider

```bash
# check registered provider
lavad query pairing account-info <lava@1sdx...>

# check all providers in LAV1 network
lavad query pairing providers LAV1
```

## Setting rpcprovider.yml

**Create a config with the following content**

```bash
nano $HOME/.lava/config/rpcprovider.yml
```

Use exactly 0.0.0.0:12345 to successfully catch incoming traffic. Also in the config, if necessary, change ports 26657, 9090 and 1317 to your RPC, gRPC and API values, respectively If you run several networks at the same time, for example LAV1 and COS4, then just add additional lines to the config below with COS4

```bash
endpoints:
  - api-interface: tendermintrpc
    chain-id: LAV1
    network-address:
      address: 0.0.0.0:12345
      disable-tls: true
    node-urls:
      - url: ws://127.0.0.1:26657/websocket
      - url: http://127.0.0.1:26657
  - api-interface: grpc
    chain-id: LAV1
    network-address:
      address: 0.0.0.0:12345
      disable-tls: true
    node-urls: 
      - url: 127.0.0.1:9090
  - api-interface: rest
    chain-id: LAV1
    network-address:
      address: 0.0.0.0:12345
      disable-tls: true
    node-urls: 
      - url: http://127.0.0.1:1317
```

{% hint style="info" %}
**Starting from v1.0.2 lavap you can and should use cache**&#x20;

Lava's caching service is used to reduce costs and improve overall network performance. Both providers and consumers benefit from a caching service. Providers that have caching enabled may return responses faster than providers that do not have caching enabled&#x20;
{% endhint %}

**Create a cashe service file**

```bash
tee /etc/systemd/system/lava-cache.service > /dev/null << EOF
[Unit]
Description=Lava cache service
After=network-online.target
[Service]
User=$USER
ExecStart=$(which lavap) cache 127.0.0.1:7777 --metrics_address 127.0.0.1:5747 --log_level debug
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

```bash
systemctl daemon-reload
systemctl enable lava-cache.service
systemctl start lava-cache.service
journalctl -u lava-cache.service -f -o cat
```



**Create a service file. Replace \<name\_wallet> with your value**

```bash
tee /etc/systemd/system/provider-lava.service > /dev/null <<EOF
[Unit]
Description=Provider Lava
After=network-online.target

[Service]
User=$USER
WorkingDirectory=/root/.lava/config
ExecStart=/root/go/bin/lavap rpcprovider lava-provider.yml --geolocation 2 --from <name_wallet> --chain-id lava-testnet-2 --keyring-backend test --cache-be 127.0.0.1:7777
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```bash
systemctl daemon-reload
systemctl enable lava-provider
systemctl restart lava-provider && journalctl -u lava-provider -f -o cat
```

![](<../../.gitbook/assets/image (28).png>)

**You can also use the configuration check command**

```bash
lavad test rpcprovider --from <name_wallet> --keyring-backend test
```

![](<../../.gitbook/assets/image (43).png>)

## Useful Commands

```
# check logs for successful transactions
journalctl -fn 100 -u lava-provider | grep succeeded

# check registered provider
lavad query pairing account-info <lava@1sdx...>

# unstake
lavad tx pairing unstake-provider "LAV1" \
    --from "<name_wallet>" \
    --keyring-backend "test" \
    --gas="auto" \
    --gas-adjustment "1.5" \
    --fees 5000ulava

# remove provider
systemctl stop lava-provider
systemctl disable lava-providerd
rm /etc/systemd/system/lava-provider.service
systemctl daemon-reload
cd $HOME/.lava/config/
rm rpcprovider.yml
```
