---
hidden: true
---

# 📬 Providers

**Details**

* Network Chain ID: `arkeo`
* Denom: `uarkeo`
* Binary: `arkeod`
* Working directory: `.arkeo`
* RPC: [https://t-arkeo.rpc.utsa.tech/](https://t-arkeo.rpc.utsa.tech/)
* RPC ETH: [https://chainlist.org/chain/1](https://chainlist.org/chain/1)
* API: [https://t-arkeo.api.utsa.tech/](https://t-arkeo.api.utsa.tech/)
* Explorers: [https://exp.utsa.tech/arkeo-test/staking](https://exp.utsa.tech/arkeo-test/staking)
* Docs: [https://docs.arkeo.network/architecture/providers](https://docs.arkeo.network/architecture/providers)

A provider can be registered or deregistered through a bonding transaction and an unbonding transaction, respectively. The provider requires a minimum amount of collateral in order for users and decentralized applications to enter into contracts with it. At the time of writing this article, this amount is equal to 1 arkeo

Providers can unbond at any time by specifying a negative amount in the transaction. If after the transaction the provider's collateral falls below the minimum required amount, then clients will no longer be able to open new contracts with him. However, any existing contracts will remain in effect until they expire or are canceled by the provider or customer

{% hint style="info" %}
To bind a provider (bond) or unbond a provider (unbond), pass the MsgBondProvider transaction

commits\_bit\_array'
{% endhint %}

## Preparing the server

```bash
apt update && apt upgrade -y

apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

**install GO**

```bash
ver="1.20.3" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

**install docker + docker-compose**

```bash
# https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-ru
apt update && \
apt install apt-transport-https ca-certificates curl software-properties-common -y && \
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && \
apt update && \
apt-cache policy docker-ce && \
sudo apt install docker-ce -y && \
docker --version
```

```bash
curl -L "https://github.com/docker/compose/releases/download/v2.10.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
ln -sf /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
```

## Setting Arkeo node

In order to start the provider we need:

* Install and synchronize the Arkeo node
* The indexing and pruning settings on the node can be any
* Create a new wallet and top it up with at least 2 coins
* Get the pubkey of our wallet in the format tarkeopub1ad... We will need to insert it into the sentinel configuration and send the necessary transactions

```bash
arkeod keys add arkeo_provider_wallet --keyring-backend os

arkeod q bank balances <address>
```

## Setting sentinel

{% hint style="info" %}
Sentinel is a purpose-built reverse proxy (similar to nginx). Its role is to be front-end services that forward requests to back-end services (i.e. RPC). It is also responsible for authentication/authorization as well as rate limiting services
{% endhint %}

**Create a directory and go to it**

```bash
cd
mkdir -p arkeo-provider && cd arkeo-provider
```

**Create docker-compose.yml**

{% hint style="warning" %}
In the config we change PROVIDER\_PUBKEY and ETH\_MAINNET\_FULLNODE It is not necessary to use ETH\_MAINNET\_FULLNODE, instead you can take the RPC of any other supported network. List of supported networks - https://github.com/arkeonetwork/arkeo/blob/master/common/service.go#L20
{% endhint %}

```bash
nano docker-compose.yml
```

```bash
version: "3"
services:
  sentinel:
#    image: ghcr.io/arkeonetwork/arkeo:latest
    image: ghcr.io/arkeonetwork/arkeo@sha256:6682680655759664095eb8eddf38950b923e3f7cd9501028e98a1449a8e88f68
    container_name: sentinel
    environment:
      NET: "mainnet"
      MONIKER: "n/a"
      WEBSITE: "n/a"
      DESCRIPTION: "n/a"
      LOCATION: "Europe"
      FREE_RATE_LIMIT: 10
      PROVIDER_PUBKEY: "<tarkeopub1add...>"
      SOURCE_CHAIN: "127.0.0.1:1317"
      EVENT_STREAM_HOST: "127.0.0.1:26657"
      CLAIM_STORE_LOCATION: "${HOME}/.arkeo/claims"
      CONTRACT_CONFIG_STORE_LOCATION: "${HOME}/.arkeo/contract_configs"
      ETH_MAINNET_FULLNODE: "<https://ethereum.mainnet.fi>"
    network_mode: host
    entrypoint: sentinel
```

```bash
docker-compose up -d
```

```bash
docker-compose logs -f --tail 100
```

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Once running we can check our sentinel via **http://\<IP\_SERVER>:3636/metadata.json**
{% endhint %}

**Getting our PubKey**

```bash
RAWPUBKEY=$(arkeod keys show arkeo_provider_wallet -p | jq -r .key) &&\
PUBKEY=$(arkeod debug pubkey-raw $RAWPUBKEY | grep 'Bech32 Acc:' | sed "s|Bech32 Acc: ||g") &&\
echo $PUBKEY
```

**Enter the missing variables and send the BOND transaction**

{% hint style="info" %}
You should do this for every service you intend to run (e.g. Bitcoin, Ethereum, Gaia, etc.)
{% endhint %}

```bash
SERVICE=eth-mainnet-fullnode
BOND=1000000
```

```bash
arkeod tx arkeo bond-provider -y --from arkeo_provider_wallet --fees 200uarkeo -- "$PUBKEY" "$SERVICE" "$BOND"
```

{% hint style="success" %}
Now we can check our provider with the command. First replace **\<tarkeopub1...>** with your value

`curl -s "https://t-arkeo.api.utsa.tech/arkeo/providers" | jq '.provider[]|select(.pub_key=="<tarkeopub1...>")'`

![](<../../.gitbook/assets/image (60).png>)
{% endhint %}

## Provider modification

Don't forget to replace the variables with your values! replace with your values https://docs.arkeo.network/how-to-use/data-providers/management#modifying

```shell
PUBKEY=<tarkeopub1...>
SERVICE=eth-mainnet-fullnode
METADATAURI=http://arkeo-provider.utsa.tech:3636/metadata.json
METADATANONCE=1
STATUS=1
MIN_CONTRACT_DURATION=5
MAX_CONTRACT_DURATION=432000
SUBSCRIPTION_RATE=10uarkeo
PAY_AS_YOU_GO_RATE=10uarkeo
SETTLEMENT_DURATION=1000
```

```bash
arkeod tx arkeo mod-provider -y --from arkeo_provider_wallet --fees 200uarkeo -- "$PUBKEY" "$SERVICE" "$METADATAURI" $METADATANONCE $STATUS $MIN_CONTRACT_DURATION $MAX_CONTRACT_DURATION $SUBSCRIPTION_RATE  $PAY_AS_YOU_GO_RATE $SETTLEMENT_DURATION
```

{% hint style="success" %}
The provider state should now change. First replace \<tarkeopub1...> with your value The provider state should now change. First replace \<tarkeopub1...> with your value

`curl -s "https://t-arkeo.api.utsa.tech/arkeo/providers" | jq '.provider[]|select(.pub_key=="<tarkeopub1...>")'`

![](<../../.gitbook/assets/image (61).png>)
{% endhint %}

At this point your provider should be working, if someone creates a contract with your provider then rewards can be branded. **Currently there are not enough contracts for all providers, so each of you can create a contract yourself and test the overall operation!**

## Useful commands

```bash
cd arkeo-provider

docker-compose up -d

docker-compose logs -f --tail 100

docker-compose restart

docker-compose stop
```

**Check all providers**

```bash
curl -s "https://t-arkeo.api.utsa.tech/arkeo/providers" | jq
```

**Check a specific provider**

```bash
curl -s "https://t-arkeo.api.utsa.tech/arkeo/providers" | jq '.provider[]|select(.pub_key=="tarkeopub1addwnpepq2gehntdwz26s6jhzmfv89enucmx7dls76zzkmq4hw5mwxdzkplk75ke05z")'
```

**Check metadata for a specific provider**

```bash
curl -s http://arkeo-provider.utsa.tech:3636/metadata.json | jq
```

```bash
curl -sX POST -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' http://arkeo-provider.utsa.tech:3636/eth-mainnet-fullnode|jq
```

**Check active contracts**

```bash
curl -s http://<ip-address>:3636/active-contract/<service>/<spender_pubkey> | jq
```

**Claims**

```bash
# 
curl -s http://<ip-address>:3636/claim/<contract_id> | jq
# open for claim
curl -s http://<ip-address>:3636/open-claims | jq
```

**Unbond**

```bash
RAWPUBKEY=$(arkeod keys show arkeo_provider_wallet -p | jq -r .key) &&\
PUBKEY=$(arkeod debug pubkey-raw $RAWPUBKEY | grep 'Bech32 Acc:' | sed "s|Bech32 Acc: ||g") &&\
echo $PUBKEY

SERVICE=eth-mainnet-fullnode
BOND=-1000000
arkeod tx arkeo bond-provider -y --from arkeo_provider_wallet --fees 200uarkeo -- "$PUBKEY" "$SERVICE" "$BOND"
```

