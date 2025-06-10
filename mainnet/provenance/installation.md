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
ver="1.23.3" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

## Node installation

```shell
export PIO_HOME=~/.provenanced
git clone https://github.com/provenance-io/provenance.git && cd provenance
git checkout tags/v1.24.0 -b v1.24.0
make golangci-lint
make clean
make install
provenanced version --long | grep -e version -e commit
# v1.24.0
# commit: ca136533
```

#### We initialize the node to create the necessary configuration files

```shell
provenanced init UTSA_guide --chain-id pio-mainnet-1
```

#### Download Genesis

```shell
wget -O $HOME/.provenanced/config/genesis.json "https://raw.githubusercontent.com/provenance-io/mainnet/main/pio-mainnet-1/genesis.json"

sha256sum ~/.provenanced/config/genesis.json
# 8b10448662a55462fb23a8f4faedae2ec8a24aefe04d2e7475400c0367948940
```

#### Set up node configuration

```
timeout_commit = "1s"
```

```shell
provenanced config set chain-id pio-mainnet-1
provenanced config set keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"1905nhash\"/;" ~/.provenanced/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.provenanced/config/config.toml
peers="b75bb5d0c033b5a8ca24df607a757d09e4f99acd@provenance-mainnet-peer.itrocket.net:57656,a4921224b51c341585372284b3dbe2bfcec8657c@213.133.100.93:27056"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.provenanced/config/config.toml
seeds="a280ec7a1b563cb71510723b860ed37d40494308@provenance-mainnet-seed.itrocket.net:57656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.provenanced/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_keep_every="0"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.provenanced/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.provenanced/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.provenanced/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.provenanced/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.provenanced/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.provenanced/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/provenanced.service > /dev/null <<EOF
[Unit]
Description=provenanced
After=network-online.target

[Service]
User=$USER
Environment="PIO_HOME=$HOME/.provenanced"
ExecStart=$(which provenanced) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable provenanced
systemctl restart provenanced && journalctl -u provenanced -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
