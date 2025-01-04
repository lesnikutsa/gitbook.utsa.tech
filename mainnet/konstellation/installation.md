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
git clone https://github.com/konstellation/konstellation && cd konstellation
git checkout v0.6.2
make build
mv $HOME/konstellation/build/knstld $HOME/go/bin/knstld

knstld version --long | grep -e version -e commit
# version: 0.6.2
# commit: 371486832893d7dc99a68ee333ca4d2e2c375f9e
```

#### We initialize the node to create the necessary configuration files

```shell
knstld init UTSA_guide --chain-id darchub
```

#### Download Genesis

```shell
wget -O $HOME/.knstld/config/genesis.json "https://share106-7.utsa.tech/konstellation/genesis.json"

sha256sum ~/.knstld/config/genesis.json
# f0e98b0749801ba0b2ea5f1eeff72df847ec78bde9b8ff9ca114bedc8afd8763
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.knstld/config/addrbook.json "https://share106-7.utsa.tech/konstellation/addrbook.json"
```

#### Set up node configuration

```shell
knstld config chain-id darchub
knstld config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.001udarc\"/;" ~/.knstld/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.knstld/config/config.toml

peers="3bf94a04c111992065810fe9e85d9adb5132b824@65.108.199.222:26626,e5e1f21957a07f623fb310737205b5b6cb1fce42@8.52.201.252:60756"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.knstld/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="nothing"
pruning_keep_recent="0"
pruning_interval="0"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.knstld/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.knstld/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.knstld/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.knstld/config/config.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/knstld.service > /dev/null <<EOF
[Unit]
Description=knstld
After=network-online.target

[Service]
User=$USER
ExecStart=$(which knstld) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable knstld
systemctl restart knstld && journalctl -u knstld -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
