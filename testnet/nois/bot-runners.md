# 📬 Bot Runners

* Official guide - https://docs.nois.network/use-cases/for-bot-runners
* Telemetry - [https://testnet.randomness.nois.network/](https://testnet.randomness.nois.network/)

{% hint style="info" %}
**New versions of the bot are divided into groups A and B - read more here**
{% endhint %}

{% hint style="warning" %}
**If you open your RPC, but indexing must be enabled on the node!**
{% endhint %}



### Bot launch

```shell
# create a separate wallet
noisd keys add botwallet

#Make sure you have tokens in your wallet
export MONIKER=your-beautiful-name
export MNEMONIC='<YOUR_MNEMONICS_HERE>'
#check https://docs.nois.network/networks-and-contracts. nois-oracle contract
export NOIS_CONTRACT=nois16peq3sftghumkja7nu32ztjy0ew4vsnshxfhcv6sxq573ta08gwsgldepm
export ENDPOINT=http://rpc-3.noislabs.com:26657
#edit above values before running the docker
docker run -d --name drandbot \
       -e MONIKER=$MONIKER \
       -e "MNEMONIC=$MNEMONIC" \
       -e PREFIX=nois \
       -e DENOM=unois \
       -e NOIS_CONTRACT=$NOIS_CONTRACT \
       -e ENDPOINT=$ENDPOINT \
       -e GAS_PRICE=0.05unois \
       noislabs/nois-bot:lates
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
