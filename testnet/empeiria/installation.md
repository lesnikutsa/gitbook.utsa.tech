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
ver="1.22.3"
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
#mkdir -p ~/go/bin
mkdir -p empe-chain && cd empe-chain
curl -LO https://github.com/empe-io/empe-chain-releases/raw/master/v0.3.0/emped_v0.3.0_linux_amd64.tar.gz
tar -xvf emped_v0.3.0_linux_amd64.tar.gz
mv emped ~/go/bin
chmod u+x ~/go/bin/emped

emped version --long | grep -e version -e commit
# version: v0.3.0
# commit: 8d602e7c6502db75b47369bbbd885cd12256e71a
```

#### We initialize the node to create the necessary configuration files

```shell
emped init UTSA_guide --chain-id empe-testnet-2
```

#### Download Genesis and Config

```shell
wget -O $HOME/.empe-chain/config/genesis.json "https://raw.githubusercontent.com/empe-io/empe-chains/master/testnet-2/genesis.json"

sha256sum ~/.empe-chain/config/genesis.json
# 2aabbc5e3cabbd0f2cdb9a442dc476397c05b624f42ee23e832fdf1c48f60c91
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.empe-chain/config/addrbook.json "https://share102.utsa.tech/empeiria/addrbook.json"
```

#### Set up node configuration

```shell
emped config chain-id empe-testnet-2 
emped config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025uempe\"/;" ~/.empe-chain/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.empe-chain/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.empe-chain/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.empe-chain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.empe-chain/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.empe-chain/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.empe-chain/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/emped.service > /dev/null <<EOF
[Unit]
Description=emped
After=network-online.target

[Service]
User=$USER
ExecStart=$(which emped) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable emped
systemctl restart emped && journalctl -u emped -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
