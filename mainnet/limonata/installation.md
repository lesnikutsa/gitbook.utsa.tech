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
ver="1.24.5"
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
mkdir -p $HOME/.limonata-mainnet

git clone https://github.com/Limonata-Blockchain/limonata.git && cd limonata
git checkout v0.4.0-mainnet.3
make install
mv $HOME/go/bin/evmd $HOME/go/bin/limonatad

limonatad version --long --home "$HOME/.limonata-mainnet"
# version: v0.4.0-mainnet.3+src.c43879f764ef
# commit: d8e530af9d3a35ba2684a8700a464e08b63220bd-modified
```

```shell
limonatad init UTSA_guide --chain-id limonata_20777-1 --home "$HOME/.limonata-mainnet"
```

#### Download Genesis

```shell
wget -O $HOME/.limonata-mainnet/config/genesis.json "https://share101.utsa.tech/limonata/genesis.json"

sha256sum ~/.limonata-mainnet/config/genesis.json
#944ad7370836c7a6f185002f6d599abe29f6cf7ce6ad30c83800537dc8d03460
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.limonata-mainnet/config/addrbook.json "https://share101.utsa.tech/limonata/addrbook.json"
```

#### Set up node configuration

Copy app.toml and config.toml from the archive at https://github.com/Limonata-Blockchain/limonata/releases/tag/v0.4.0-mainnet.3&#x20;

```shell
sed -i.bak -e "s/^chain-id *=.*/chain-id = \"limonata_20777-1\"/;" ~/.limonata-mainnet/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"os\"/;" ~/.limonata-mainnet/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0aLIMO\"/;" ~/.limonata-mainnet/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.limonata-mainnet/config/config.toml
peers="4adb8c1541f90d2548a30242a24d42f9294aa121@95.217.62.179:26656,ae70458af1a3f05d7e8b582b4f40b8716789365e@135.181.232.179:26656,81ca70f18a50c8be7e3e33e6f7703082c72ea00c@152.53.87.42:26656,61c05efac48b5131b65b1d69d7f4e47967d0a557@135.181.134.96:26656,57bdc74b5ff5c82b76bf3ce1c29857e50698c1ea@65.109.106.214:26656,b6f04d03fa974501849fff3c524614d36bae4b8a@65.21.227.52:26656,e4fac15d52839da6994dcd766b2305640e44dc1e@117.1.193.44:26656,0c2328296e393530487bdb0eef34b746d9458c5e@65.109.32.61:12456,c41b9216a40377d1ecbb4c00170ee959443dd47d@152.53.183.231:26656,a6e2f2fdf56b7e700e4684cf629ef0a30a5153e5@65.108.70.106:26656,a1138afc61dc7465c45294ab53ea22e477a8153b@116.202.218.189:26656,23069f0d2701ad41acdc54766e9295b527b6dedc@65.109.55.116:26656,67daba23c58a1cc39d12d836e495a3d976414741@65.109.57.54:26656,117d80a2eff94eda36762ddda510fd5f254d5a72@159.195.204.61:26656,f4317ea1b34a55dd8c3418a7327ada632075fe88@65.108.206.118:26656,e5b7e4eda3a82c96f13150419bb3b2b0091b8384@65.21.234.111:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.limonata-mainnet/config/config.toml
seeds=""
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.limonata-mainnet/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.limonata-mainnet/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="100"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.limonata-mainnet/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.limonata-mainnet/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.limonata-mainnet/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.limonata-mainnet/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.limonata-mainnet/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/limonatad.service > /dev/null <<EOF
[Unit]
Description=limonatad
After=network-online.target

[Service]
User=$USER
ExecStart=$(which limonatad) start --home $HOME/.limonata-mainnet --keyring-backend memory
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable limonatad
systemctl restart limonatad && journalctl -u limonatad -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
