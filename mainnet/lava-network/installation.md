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
git checkout v4.2.0
make install-all

lavad version --long | grep -e version -e commit
#version: 4.2.0
#commit: 18c395ba01dbd650eac3d276991782faa28270ae
```

#### We initialize the node to create the necessary configuration files

```shell
lavad init UTSA_guide --chain-id lava-mainnet-1
```

#### Download Genesis

```shell
wget -O $HOME/.lava/config/genesis.json "https://storage.googleapis.com/lavanet-public-asssets/tge/genesis.json"

sha256sum ~/.lava/config/genesis.json
# 5ba7dc9ac4ee0ea1776c8f13dc45be84ffc2ef3ffbe8271e30b573134ecd61ea
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.lava/config/addrbook.json "https://share.utsa.tech/lava/addrbook.json"
```

#### Set up node configuration

```shell
lavad config chain-id lava-mainnet-1
lavad config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.000000001ulava\"/;" ~/.lava/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.lava/config/config.toml
seeds="ebacd3e666003397fb685cd44956d33419219950@seed2.lava.chainlayer.net:26656,f1caeaacfac32e4dd00916e8d912e1d834e94eb3@lava-seed.stakecito.com:26666,e4eb68c6fdfab1575b8794205caed47d4f737df4@lava-mainnet-seed.01node.com:26107"
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
