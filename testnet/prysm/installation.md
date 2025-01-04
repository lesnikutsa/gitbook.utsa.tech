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
git clone https://github.com/kleomedes/prysm && cd prysm
git checkout v0.1.0-devnet
make install

prysmd version --long | grep -e version -e commit
# version: 0.1.0-devnet
# commit: f48a40b9541508c19b037e1a659c7ab048a1946d
```

#### We initialize the node to create the necessary configuration files

```shell
prysmd init UTSA_guide --chain-id prysm-devnet-1
```

#### Download Genesis

```shell
wget -O $HOME/.prysm/config/genesis.json "https://snapshots.polkachu.com/testnet-genesis/prysm/genesis.json"

sha256sum ~/.prysm/config/genesis.json
# afe3fae047851409fa4e8bbb321a723fb4b2dc783bb5079c716bdf7df336e0c4
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.prysm/config/addrbook.json "https://share101.utsa.tech/prysm/addrbook.json"
```

#### Set up node configuration

```shell
prysmd config set client chain-id prysm-devnet-1 
prysmd config set client keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.001uprysm\"/;" ~/.prysm/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.prysm/config/config.toml
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.prysm/config/config.toml
seeds="ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@testnet-seeds.polkachu.com:29856"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.prysm/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.prysm/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.prysm/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.prysm/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.prysm/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.prysm/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/prysmd.service > /dev/null <<EOF
[Unit]
Description=prysmd
After=network-online.target

[Service]
User=$USER
ExecStart=$(which prysmd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable prysmd
systemctl restart prysmd && journalctl -u prysmd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
