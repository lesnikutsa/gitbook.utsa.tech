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
mkdir -p $HOME/latanda-build && cd $HOME/latanda-build

wget -q https://latanda.online/chain/latanda-chain-source.tar.gz
tar -xzf latanda-chain-source.tar.gz

go mod tidy
go build -o latandad ./cmd/latandad

mv latandad $HOME/go/bin/

latandad version --long | grep -e version -e commit
# version: ""
# commit: "" 
```

#### We initialize the node to create the necessary configuration files

```shell
latandad init UTSA_guide --chain-id latanda-testnet-1 --default-denom ultd
```

#### Download Genesis

```shell
wget -O $HOME/.latanda/config/genesis.json "https://latanda.online/chain/genesis.json"

sha256sum ~/.latanda/config/genesis.json
# 98fc9871d6a3b7b12b3f7fcaa1ca3303ffcfad0f209d61355975a15069ac3907
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.latanda/config/addrbook.json "https://share101.utsa.tech/latanda/addrbook.json"
```

#### Set up node configuration

```shell
sed -i.bak -e "s/^chain-id *=.*/chain-id = \"latanda-testnet-1\"/;" ~/.latanda/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"os\"/;" ~/.latanda/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.001ultd\"/;" ~/.latanda/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.latanda/config/config.toml
peers="483a8110c3cd93c8dd3801d935151e98656f5b67@168.231.67.201:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.latanda/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.latanda/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.latanda/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.latanda/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.latanda/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.latanda/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.latanda/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/latandad.service > /dev/null <<EOF
[Unit]
Description=latandad
After=network-online.target
Wants=network-online.target

[Service]
User=$USER
ExecStart=$(which latandad) start --home $HOME/.latanda
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable latandad
systemctl restart latandad && journalctl -u latandad -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
