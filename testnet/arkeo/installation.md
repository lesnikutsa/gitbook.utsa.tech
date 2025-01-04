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
git clone https://github.com/arkeonetwork/arkeo && cd arkeo
git checkout master
TAG=testnet make install

arkeod version --long | grep -e version -e commit
# version: v1.0.0-prerelease
# commit: 51e568dd1cff1af6249fa3b4806ccd5573249ae2
```

#### We initialize the node to create the necessary configuration files

```shell
arkeod init UTSA_guide --chain-id arkeo-testnet-3
```

#### Download Genesis

```shell
curl -s http://seed31.innovationtheory.com:26657/genesis | jq '.result.genesis' > $HOME/.arkeo/config/genesis.json

sha256sum ~/.arkeo/config/genesis.json
# 1e8c6c54bdfa3da1587967806cd9b9092262521e230063a2a4e672b8d390abe7
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.arkeo/config/addrbook.json "https://share101.utsa.tech/arkeo/addrbook.json"
```

#### Set up node configuration

```shell
arkeod config set client chain-id arkeo-testnet-3
arkeod config set client keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.001uarkeo\"/;" ~/.arkeo/config/app.toml

# seeds/peers
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.arkeo/config/config.toml
peers="bb761c984bd990f3055f412917396754cd22af7a@validator31.innovationtheory.com:26656,81e36f94351d47803b8e1e0d0ad2d2e8e14ed36b@validator32.innovationtheory.com:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.arkeo/config/config.toml
seeds="9dfa5f2d19c1174baf5e597965394269e654f9b7@seed31.innovationtheory.com:26656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.arkeo/config/config.toml

sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.arkeo/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.arkeo/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.arkeo/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.arkeo/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.arkeo/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.arkeo/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/arkeod.service > /dev/null <<EOF
[Unit]
Description=arkeod
After=network-online.target

[Service]
User=$USER
ExecStart=$(which arkeod) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable arkeod
systemctl restart arkeod && journalctl -u arkeod -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
