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
ver="1.20.5"
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
git clone https://github.com/lavanet/lava && cd lava
mkdir -p $HOME/lava/build
cd $HOME/lava/build

wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v4.2.0/lavad-v4.2.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
mv $HOME/lava/build/lavad $(which lavad)

lavad version --long | grep -e version -e commit
#version: 4.2.0
#commit: 18c395ba01dbd650eac3d276991782faa28270ae
```

#### We initialize the node to create the necessary configuration files

```shell
lavad init UTSA_guide --chain-id lava-testnet-2
```

#### Download Genesis

```shell
wget -O $HOME/.lava/config/genesis.json "https://raw.githubusercontent.com/lavanet/lava-config/main/testnet-2/genesis_json/genesis.json"

# Проверим генезис
sha256sum ~/.lava/config/genesis.json
# f7a0c7d2587d2bf640570309137c905eac834f0aba99f90b4c10f45ef8334583
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.lava/config/addrbook.json "https://share101.utsa.tech/lava/addrbook.json"
```

#### Set up node configuration

```shell
lavad config chain-id lava-testnet-2
lavad config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0ulava\"/;" ~/.lava/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.lava/config/config.toml
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.lava/config/config.toml
seeds="3a445bfdbe2d0c8ee82461633aa3af31bc2b4dc0@testnet2-seed-node.lavanet.xyz:26656,e593c7a9ca61f5616119d6beb5bd8ef5dd28d62d@testnet2-seed-node2.lavanet.xyz:26656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.lava/config/config.toml

sed -i \
-e 's/timeout_propose = .*/timeout_propose = "1s"/' \
-e 's/timeout_propose_delta = .*/timeout_propose_delta = "500ms"/' \
-e 's/timeout_prevote = .*/timeout_prevote = "1s"/' \
-e 's/timeout_prevote_delta = .*/timeout_prevote_delta = "500ms"/' \
-e 's/timeout_precommit = .*/timeout_precommit = "500ms"/' \
-e 's/timeout_precommit_delta = .*/timeout_precommit_delta = "1s"/' \
-e 's/timeout_commit = .*/timeout_commit = "15s"/' \
-e 's/^create_empty_blocks = .*/create_empty_blocks = true/' \
-e 's/^create_empty_blocks_interval = .*/create_empty_blocks_interval = "15s"/' \
-e 's/^timeout_broadcast_tx_commit = .*/timeout_broadcast_tx_commit = "151s"/' \
-e 's/skip_timeout_commit = .*/skip_timeout_commit = false/' \
$HOME/.lava/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.lava/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.lava/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.lava/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.lava/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.lava/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/lavad.service > /dev/null <<EOF
[Unit]
Description=lavad
After=network-online.target

[Service]
User=$USER
ExecStart=$(which lavad) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable lavad
systemctl restart lavad && journalctl -u lavad -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
