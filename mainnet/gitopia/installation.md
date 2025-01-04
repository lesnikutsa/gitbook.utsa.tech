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
git clone https://github.com/gitopia/gitopia && cd gitopia
git checkout v5.1.0
make install

gitopiad version --long | grep -e version -e commit -e build
# version: 5.1.0
# commit: 3b1ea32980e79ea74a1398f78a0388867e8d5fd4
# build_tags: netgo,ledger
```

#### We initialize the node to create the necessary configuration files

```shell
gitopiad init UTSA_guide --chain-id gitopia
```

#### Download Genesis

```shell
cd && git clone https://github.com/gitopia/mainnet && cd mainnet
tar -xzf genesis.tar.gz
shasum -a 256 genesis.json
# 0cf5c55e6ea1fbcebccadba0f6dc0b83ac76d1b608487a06978956404ce33e66

# копируем genesis
cp genesis.json $HOME/.gitopia/config/genesis.json
gitopiad validate-genesis
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.gitopia/config/addrbook.json "https://share.utsa.tech/gitopia/addrbook.json"
```

#### Set up node configuration

```shell
gitopiad config chain-id gitopia 
gitopiad config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025ulore\"/;" ~/.gitopia/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.gitopia/config/config.toml
peers="b35d46fcfc1e4cfa943a299fcb39853e15e94d8b@81.30.157.35:14656,4cf66531681c92f15c95c25bd1bff524f9dca35e@65.109.154.181:26656,b2f764694d52e09793d68259d584ece0c194b6fe@65.108.229.93:26656,082e95b5d5351e68dcfb24dff802f9064cfd5a4c@65.109.92.241:51056,a0ebd1e5845148c47451452047c7c99621da195e@65.109.96.93:60556,4adfa5889675e1e91ea4459e15ff4a0ba53e7828@65.108.224.156:19656,12f6b84a23b054a6591c647c2a4456c40af65cce@5.9.147.22:24657,88497ab3bbbcc1e8545771f45020e738bcce590f@95.165.89.222:24136,abca18ed112719b4f0a23932797dba2733f0fd44@23.88.5.169:25656,976d95adec7f0d7fda4464df019fa538fa0bb4ce@144.76.97.251:44656,ffd761a9e0d86609de6dae5935f99451694051a9@34.28.130.17:26656,5b2df98ad73a0a81a5bd31da4489a9236a7d7a99@65.21.91.160:26867,712dd67b7abe08577d394e90a4930492c8f7d2ee@65.108.124.219:41656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.gitopia/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.gitopia/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.gitopia/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.gitopia/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.gitopia/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.gitopia/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/gitopiad.service > /dev/null <<EOF
[Unit]
Description=gitopiad
After=network-online.target

[Service]
User=$USER
ExecStart=$(which gitopiad) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable gitopiad
systemctl restart gitopiad && journalctl -u gitopiad -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
