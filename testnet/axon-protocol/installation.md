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
ver="1.23.1"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```

## Node installation

#### Setup validator name

```shell
MONIKER="YOUR_MONIKER_HERE"
```

```shell
git clone https://github.com/axone-protocol/axoned && cd axoned
git checkout v10.0.0
make install

axoned version --long | grep -e version -e commit
#version: 10.0.0
#commit: 2f0f84d369852bdb178e299a88c1b8eeb0654b8e
```

#### We initialize the node to create the necessary configuration files

```shell
axoned init <name_moniker> --chain-id axone-dentrite-1
```

#### Download Genesis

```shell
wget -O $HOME/.axoned/config/genesis.json "https://raw.githubusercontent.com/axone-protocol/networks/main/chains/dentrite-1/genesis.json"

# Проверим генезис
sha256sum ~/.axoned/config/genesis.json
# da1434f8dd7692f093808676e6a43ac37b25ba22fd7c893ac855da2445dc3ec6
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.axoned/config/addrbook.json ""
```

#### Set up node configuration

```shell
axoned config chain-id axone-dentrite-1
axoned config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025uaxone\"/;" ~/.axoned/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.axoned/config/config.toml
peers="d89568d0fda69b1951a433f5f5ff887213a41305@5.9.73.170:17656,72ddc04c09160a58289f9a7c5b29aac358635178@142.132.202.50:38656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.axoned/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.axoned/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.axoned/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.axoned/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.axoned/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.axoned/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/axoned.service > /dev/null <<EOF
[Unit]
Description=axoned
After=network-online.target

[Service]
User=$USER
ExecStart=$(which axoned) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable axoned
systemctl restart axoned && journalctl -u axoned -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
