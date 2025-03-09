---
description: 'Officially stated requirements: 2/4/100 ubuntu 22.04'
---

# 💻 Light node

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev libgmp3-dev tar clang bsdmainutils ncdu unzip llvm libudev-dev make protobuf-compiler -y
```

```bash
ver="1.22.1" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

## Installing Ubuntu 22.04

{% hint style="info" %}
Light node does not create transactions, but verifies and relays transactions created by wallets or other nodes
{% endhint %}

The Nubit team took care of us and Light nodes can be installed with a simple command

```shell
tmux new-session -s nubit_light_node
```

```
curl -sL1 https://nubit.sh | bash
```

If you see mnemonics and PUBKEY after launch, then Light nodes has been successfully launched

<figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
**Don't forget to save your wallet details!!!**
{% endhint %}

If you prefer manual installation, then use the official documentation - https://docs.nubit.org/nubit-da/run-a-node-advanced/make-prerequisities



{% hint style="warning" %}
**IMPORTANT - After installing light node, you must register your participation on the website&#x20;**<mark style="color:purple;">**https://alpha.nubit.org/#/**</mark>
{% endhint %}

### Useful Light node commands

```shell
# check status and blocks
$HOME/nubit-node/bin/nubit das sampling-stats --node.store $HOME/.nubit-light-nubit-alphatestnet-1

# show wallet address
$HOME/nubit-node/bin/nubit state account-address  --node.store $HOME/.nubit-light-nubit-alphatestnet-1

# show list of wallets
$HOME/nubit-node/bin/nkey list --p2p.network nubit-alphatestnet-1 --node.type light

# remove Light node
rm -rf $HOME/nubit-node
rm -rf $HOME/.nubit-light-nubit-alphatestnet-1
```

