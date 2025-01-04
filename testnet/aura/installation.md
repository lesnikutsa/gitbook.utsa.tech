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

#### Setup validator name

```shell
MONIKER="YOUR_MONIKER_HERE"
```

```shell
git clone https://github.com/aura-nw/aura && cd aura
wget https://github.com/aura-nw/aura/releases/download/v0.9.3-euphoria/aurad
chmod +x aurad
mv aurad $(which aurad)
aurad version --long | grep -e commit -e version
#version: v0.9.3-euphoria
#commit: d3e717e44a2b9fadf3c19ec6a8bc2a98e2bac470
```

#### We initialize the node to create the necessary configuration files

```shell
aurad init $MONIKER --chain-id aura_6321-3
```

#### Download Genesis

```shell
wget https://aura-network.s3.ap-southeast-1.amazonaws.com/aura_6321-3-genesis.tar.gz
tar -xzvf aura_6321-3-genesis.tar.gz
mv aura_6321-3-genesis.json $HOME/.aura/config/genesis.json

# Verify genesis info
cat $HOME/.aura/config/genesis.json | jq '"Genesis Time: " + .genesis_time + " — Chain ID: " + .chain_id + " - Initial Height: " + .initial_height'
# this should return
# "Genesis Time: 2024-04-23T13:30:00Z — Chain ID: aura_6321-3 - Initial Height: 9993651"

# Verify sorted shasum
jq -S -c -M '' $HOME/.aura/config/genesis.json | sha256sum
# this should return
# 26311ac3110258d26f8999b92c06f3d3a9588fef2224197a9e6d04a0354eee5e  -
```

#### At this stage, we can download the address book

```shell
```

#### Set up node configuration

```shell
aurad config chain-id aura_6321-3
aurad config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025ueaura\"/;" ~/.aura/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.aura/config/config.toml
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.aura/config/config.toml
seeds="705e3c2b2b554586976ed88bb27f68e4c4176a33@52.76.203.126:26656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.aura/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.aura/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.aura/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.aura/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.aura/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.aura/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/aurad.service > /dev/null <<EOF
[Unit]
Description=aurad
After=network-online.target

[Service]
User=$USER
ExecStart=$(which aurad) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable aurad
systemctl restart aurad && journalctl -u aurad -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
