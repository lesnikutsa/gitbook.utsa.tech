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
git clone https://github.com/noislabs/noisd && cd noisd
git checkout v1.0.4
make install

noisd version --long |  grep -e version -e commit -e build
# version: 1.0.4
# commit: 1d5905dbbe6fcf757026abe8110786fd9746c1ec
# build_tags: netgo,ledger
```

#### We initialize the node to create the necessary configuration files

```shell
noisd init UTSA_guide --chain-id nois-testnet-005
```

#### Download Genesis

```shell
wget -O $HOME/.noisd/config/genesis.json "https://raw.githubusercontent.com/noislabs/networks/main/nois-testnet-005/genesis.json"

sha256sum ~/.noisd/config/genesis.json
# 1647cabd044110d6beda2fc4a32fb50dfbb5babff74bdf41503c58123ded3389
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.noisd/config/addrbook.json "https://raw.githubusercontent.com/obajay/nodes-Guides/main/Nois/Noist_Testnet/addrbook.json"
```

#### Set up node configuration

```shell
noisd config chain-id nois-testnet-005
noisd config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.05unois\"/;" ~/.noisd/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.noisd/config/config.toml
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.noisd/config/config.toml
seeds="bf07906c7cf0f23606c83be15624be2c67b3929c@139.59.154.47:17356"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.noisd/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.noisd/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.noisd/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.noisd/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.noisd/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.noisd/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/noisd.service > /dev/null <<EOF
[Unit]
Description=noisd
After=network-online.target

[Service]
User=$USER
ExecStart=$(which noisd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable noisd
systemctl restart noisd && journalctl -u noisd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
