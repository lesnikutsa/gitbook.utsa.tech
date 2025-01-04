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
mkdir -p $HOME/go/bin

git clone https://github.com/Galactica-corp/galactica && cd galactica
git checkout v0.2.7
make install

galacticad version --long | grep -e version -e commit
# version: 0.2.7
# commit: da411a7a648f2cdd1bf7961b35272218a0c5b92f
```

#### We initialize the node to create the necessary configuration files

```shell
galacticad init UTSA_guide --chain-id galactica_9302-1
```

#### Download Genesis and Config

```shell
wget -O $HOME/.galactica/config/genesis.json "https://raw.githubusercontent.com/Galactica-corp/networks/main/galactica_9302-1/genesis.json"

sha256sum ~/.galactica/config/genesis.json
# 03f88591a3a60c9940c7f49d43f24e8bb6e39ea4ff22d783fcc6f12762446e59
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.galactica/config/addrbook.json "https://share101.utsa.tech/warden/addrbook.json"
```

#### Set up node configuration

```shell
galacticad config chain-id galactica_9302-1
galacticad config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"10agnet\"/;" ~/.galactica/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.galactica/config/config.toml
peers=""
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.galactica/config/config.toml
seeds="52ccf467673f93561c9d5dd4434def32ef2cd7f3@galactica-testnet-seed.itrocket.net:46656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.galactica/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.galactica/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.galactica/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.galactica/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.galactica/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.galactica/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.galactica/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/galacticad.service > /dev/null <<EOF
[Unit]
Description=galacticad
After=network-online.target

[Service]
User=$USER
ExecStart=$(which galacticad) start --chain-id galactica_9302-1
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable galacticad
systemctl restart galacticad && journalctl -u galacticad -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
