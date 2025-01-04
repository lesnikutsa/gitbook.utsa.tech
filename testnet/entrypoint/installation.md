# 💻 Installation

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

#### Install GO

```shell
ver="1.19.1"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```

## Node installation

```shell
mkdir -p $HOME/entrypoint && cd entrypoint
wget https://github.com/entrypoint-zone/testnets/releases/download/v1.2.0/entrypointd-v1.2.0-linux-amd64
chmod +x entrypointd-v1.2.0-linux-amd64
mv entrypointd-v1.2.0-linux-amd64 $HOME/go/bin/entrypointd

entrypointd version --long | grep -e version -e commit -e build
# version: 1.2.0
# commit: 296eee5efbc6c41688795333886febb67a9eeb65
# build_tags: ""
```

#### We initialize the node to create the necessary configuration files

```shell
entrypointd init UTSA_guide --chain-id entrypoint-pubtest-2
```

#### Download Genesis

```shell
wget -O $HOME/.entrypoint/config/genesis.json "https://raw.githubusercontent.com/entrypoint-zone/testnets/main/entrypoint-pubtest-2/genesis.json"

# Проверим генезис
sha256sum ~/.entrypoint/config/genesis.json
# 15ff4506d0a1a4fec33a0fef6a5d19be3c4dfbe692f1f71153dcb3eb2a2be06b
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.entrypoint/config/addrbook.json "https://share101.utsa.tech/entrypoint/addrbook.json"
```

#### Set up node configuration

```shell
entrypointd config chain-id entrypoint-pubtest-2
entrypointd config keyring-backend os
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.entrypoint/config/config.toml
peers="81bf2ade773a30eccdfee58a041974461f1838d8@185.107.68.148:26656,d57c7572d58cb3043770f2c0ba412b35035233ad@80.64.208.169:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.entrypoint/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.entrypoint/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.entrypoint/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.entrypoint/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.entrypoint/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.entrypoint/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/entrypointd.service > /dev/null <<EOF
[Unit]
Description=entrypointd
After=network-online.target

[Service]
User=$USER
ExecStart=$(which entrypointd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable entrypointd
systemctl restart entrypointd && journalctl -u entrypointd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
