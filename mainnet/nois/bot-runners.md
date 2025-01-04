# 📬 Bot Runners

* Official guide - https://docs.nois.network/use-cases/for-bot-runners
* Telemetry - https://randomness.nois.network

{% hint style="info" %}
**New versions of the bot are divided into groups A and B - read more here**
{% endhint %}

{% hint style="warning" %}
**If you open your RPC, but indexing must be enabled on the node!**
{% endhint %}



### Bot launch

**Create a separate wallet and save mnemonics**

```shell
noisd keys add botwallet
```

**Create a config**

We change the config to suit ourselves. Enter your and check RPC, drandAddress and gatewayAddress - see current data here - [https://github.com/noislabs/bot2/wiki/Migrating-to-bot2](https://github.com/noislabs/bot2/wiki/Migrating-to-bot2)

```bash
nano $HOME/.noisd/config.json
```

```bash
{
  "rpcEndpoint": "https://m-nois.rpc.utsa.tech:443",
  "rpcEndpoint2": "",
  "rpcEndpoint3": "",
  "drandAddress": "nois19w26q6n44xqepduudfz2xls4pc5lltpn6rxu34g0jshxu3rdujzsj7dgu8",
  "gatewayAddress": "nois1acyc05v6fgcdgj88nmz2t40aex9nlnptqpwp5hf8hwg7rhce9uuqgqz5wp",
  "mnemonic": "<MNEMONIC>",
  "denom": "unois",
  "gasPrice": "0.05unois",
  "prefix": "nois",
  "moniker": "",
  "drandEndpoints": [
    "https://api3.drand.sh/",
    "https://drand.cloudflare.com/"
  ]
}
```

**Let's launch the bot**

```bash
docker run -d --name drandbot \
  -v "$HOME/.noisd/config.json":/app/config.json \
  noislabs/nois-bot:nextgen
```



### Useful Commands

```sh
# logs
docker logs drandbot --follow --tail=50

# restart 
docker restart drandbot && docker logs drandbot --follow --tail=50

# check containers
docker ps
docker ps -a

# update
docker stop drandbot
docker rm drandbot
docker pull noislabs/nois-bot:latest
# run
```
