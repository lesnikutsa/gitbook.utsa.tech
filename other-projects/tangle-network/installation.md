---
description: 'Officially stated requirements: 2/4/100 ubuntu 22.04'
---

# 💻 Installation

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev libgmp3-dev tar clang bsdmainutils ncdu unzip llvm libudev-dev make protobuf-compiler -y
```

## Installing Ubuntu 22.04

**Download the binary file**

```shell
mkdir -p $HOME/.tangle && cd $HOME/.tangle

wget -O tangle https://github.com/webb-tools/tangle/releases/download/v1.0.12/tangle-default-linux-amd64
chmod 744 tangle
mv tangle /usr/bin/
tangle --version
# tangle 1.0.12-ff0251a-x86_64-linux-gnu
```

**Download json**

```shell
wget -O $HOME/.tangle/tangle-mainnet.json "https://raw.githubusercontent.com/webb-tools/tangle/main/chainspecs/mainnet/tangle-mainnet.json"
chmod 744 ~/.tangle/tangle-mainnet.json
# Проверим json
sha256sum ~/.tangle/tangle-mainnet.json
# b640e7fb959066ce29a3ddece42f17cc4e76b4383fd57e4f4249e2c80bff8a00
```

### Create keys

```bash
# Acco
tangle key insert --base-path $HOME/.tangle/data/ \
--chain $HOME/.tangle/tangle-mainnet.json \
--scheme Sr25519 \
--suri "<SEED_ФРАЗА-12>" \
--key-type acco

# Babe
tangle key insert --base-path $HOME/.tangle/data/ \
--chain $HOME/.tangle/tangle-mainnet.json \
--scheme Sr25519 \
--suri "<SEED_ФРАЗА-12>" \
--key-type babe

# Imonline
tangle key insert --base-path $HOME/.tangle/data/ \
--chain $HOME/.tangle/tangle-mainnet.json \
--scheme Sr25519 \
--suri "<SEED_ФРАЗА-12>" \
--key-type imon

# Role
tangle key insert --base-path $HOME/.tangle/data/ \
--chain $HOME/.tangle/tangle-mainnet.json \
--scheme Ecdsa \
--suri "<SEED_ФРАЗА-12>" \
--key-type role

# Grandpa
tangle key insert --base-path $HOME/.tangle/data/ \
--chain $HOME/.tangle/tangle-mainnet.json \
--scheme Ed25519 \
--suri "<SEED_ФРАЗА-12>" \
--key-type gran

# check
ls $HOME/.tangle/data/chains/tangle-mainnet/keystore/
```

#### Create a service file

```
yourname=<name>
```

```shell
tee /etc/systemd/system/tangle.service > /dev/null << EOF
[Unit]
Description=Tangle Validator Node
After=network-online.target
StartLimitIntervalSec=0
[Service]
User=$USER
Restart=always
RestartSec=3
LimitNOFILE=65535
ExecStart=/usr/bin/tangle \
  --base-path $HOME/.tangle/data/ \
  --name '$yourname' \
  --chain $HOME/.tangle/tangle-mainnet.json \
  --node-key-file "$HOME/.tangle/node-key" \
  --port 30333 \
  --rpc-port 9933 \
  --prometheus-port 9615 \
  --validator \
  --pruning archive \
  --telemetry-url "wss://telemetry.polkadot.io/submit/ 1"
  --no-mdns
[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable tangle
systemctl restart tangle && journalctl -u tangle -f -o cat
```

![](<../../.gitbook/assets/image (54).png>)

Now our node has started to synchronize. We can check our node in [telemetry](https://telemetry.polkadot.io/#list/0xea63e6ac7da8699520af7fb540470d63e48eccb33f7273d2e21a935685bf1320)

## Validator setup

After the node has synchronized, we pull out the key from our node by entering the command

```bash
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9933
```

> If you get a similar result, then everything is great {"jsonrpc":"2.0","result":"**0xa0very0long0hex0string**","id":1} - copy the key (in bold) we will need it in the near future

{% hint style="warning" %}
**IMPORTANT** - save the keys located in `$HOME/.tangle/node-key` and `$HOME/.tangle/data/chains/tangle-standalone-testnet/keystore/`
{% endhint %}

* Go to the website and first create a stash wallet
* For stash we configure **Set on-chain Identity** for identification
* We create a validator. To do this, select **Network - Staking - Accounts - Validator**

![](<../../.gitbook/assets/image (55).png>)

Next, insert our key received from the validator node, select the commission percentage

![](<../../.gitbook/assets/image (56).png>)

As soon as a place among the validators becomes available, you will appear in the Staking Overview tab, but for now you can find yourself on the Waiting tab
