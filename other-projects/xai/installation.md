---
description: >-
  Officially stated requirements: 4 GB RAM 2 CPU Cores 60 GB Disk Space x86/X64
  Processor
---

# 💻 Installation

{% hint style="danger" %}
To receive rewards for running a node, you must have at least 1 Sentry key in your wallet. Keys must be purchased. More details here - [https://xai-foundation.gitbook.io/xai-network/xai-blockchain/sentry-node-purchase-and-setup/step-1-purchase-a-sentry-key](https://xai-foundation.gitbook.io/xai-network/xai-blockchain/sentry-node-purchase-and-setup/step-1-purchase-a-sentry-key)

In addition to the NFT, the Sentry wallet must be topped up with Arbitrum One ETH (AETH) to pay for the gas required to receive the reward. On average, 0.02 ETH is enough to cover the costs required to run 1 key on a node that is active 24/7 for 1 year
{% endhint %}

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

## Node installation

```shell
tmux new-session -s xai
mkdir -p $HOME/.xai
cd .xai
```

```shell
wget -O $HOME/.xai/sentry-node-cli-linux.zip "https://github.com/xai-foundation/sentry/releases/download/1.1.5/sentry-node-cli-linux.zip"
unzip sentry-node-cli-linux.zip

rm sentry-node-cli-linux.zip

sha256sum $HOME/.xai/sentry-node-cli-linux
#aafeceb73805402bd5bff43f83bd1addd3e22847f6018d22ea207e089b0495f1
```

```
./sentry-node-cli-linux
```

**Adding Sentry Wallet (operator)**

{% hint style="info" %}
When using the CLI, it is not necessary to assign your key wallets to an Operator, since when using the boot-operator command, each wallet containing a key serves as an Operator by default. However, if you want to designate a specific wallet as an Operator, you can follow these steps
{% endhint %}

```bash
# enter the command to add an operator and enter the private key of the wallet
add-operator
# look at the list of operators
list-operators
```

**To start, enter the command and enter the private key of the wallet**

```bash
boot-operator
```

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

