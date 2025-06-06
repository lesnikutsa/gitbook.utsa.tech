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
ver="1.20.3"
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
git clone -b testnet https://github.com/Source-Protocol-Cosmos/source && cd source
git checkout v3.0.0
make install

sourced version --long | head
# version: v3.0.0
# commit: 39030aa705cf18819b0a439c1bd646ed9195728c
```

#### We initialize the node to create the necessary configuration files

```shell
sourced init UTSA_guide --chain-id sourcetest-1
```

#### Download Genesis

```shell
wget -O $HOME/.source/config/genesis.json "https://raw.githubusercontent.com/Source-Protocol-Cosmos/testnets/master/sourcetest-1/genesis.json"

# Проверим генезис
sha256sum ~/.source/config/genesis.json
# c8b8e28f1cc2c6bb708d963146842da9e367874267d90ab99a13a6bd736d5682
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.source/config/addrbook.json "https://raw.githubusercontent.com/obajay/nodes-Guides/main/Projects/Source/addrbook.json"
```

#### Set up node configuration

```shell
sourced config chain-id sourcetest-1
sourced config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"1usource\"/;" ~/.source/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.source/config/config.toml
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.source/config/config.toml
seeds=""
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.source/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.source/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom" && \
pruning_keep_recent="1000" && \
pruning_keep_every="0" && \
pruning_interval="10" && \
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.source/config/app.toml && \
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.source/config/app.toml && \
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.source/config/app.toml && \
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.source/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null" && \
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.source/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000 && \
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.source/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/sourced.service > /dev/null <<EOF
[Unit]
Description=sourced
After=network-online.target

[Service]
User=$USER
ExecStart=$(which sourced) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable sourced
systemctl restart sourced && journalctl -u sourced -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
