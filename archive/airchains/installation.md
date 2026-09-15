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
mkdir -p $HOME/go/bin

mkdir -p $HOME/airchains && cd airchains

wget -O junctiond https://github.com/airchains-network/junction/releases/download/v0.3.2/junctiond-linux-amd64
chmod +x junctiond
mv $HOME/airchains/junctiond $HOME/go/bin/

junctiond version --long | grep -e version -e commit
# version: v0.3.2
# commit: e897160cebbb7ca4991353dcb6b42a571dfe793d
```

#### We initialize the node to create the necessary configuration files

```shell
junctiond init UTSA_guide --chain-id varanasi-1
```

#### Download Genesis

```shell
wget -O $HOME/.junctiond/config/genesis.json "https://raw.githubusercontent.com/airchains-network/junction-resources/refs/heads/main/varanasi-testnet/genesis/genesis.json"

sha256sum ~/.junctiond/config/genesis.json
# 2ed96ef2262d9f089a972ceaff36c416ff382ef7a26494b1d588da93713dd15c
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.junctiond/config/addrbook.json "https://share102.utsa.tech/airchains/addrbook.json"
```

#### Set up node configuration

```shell
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.00025umaf\"/;" ~/.junctiond/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.junctiond/config/config.toml
peers="cfb052e64e508f4b40f0ec07b52a31abfda82270@95.217.62.179:11956,db686fcfdf0b4676d601d5beb11faee5ad96bff1@37.27.71.199:28656,ec7d3566e70f479a4a6dd98d1e02e8827cf34f76@72.251.3.24:56256,f84b41b95e828ee915aea19dd656cca7d39cf47b@37.17.244.207:33656,306e1cc0936111831eedfa861abe3989d04acc19@158.220.114.0:26656,c0f3abcd838aeb72f6c7a1c817407bfe021547f3@135.181.139.249:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.junctiond/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.junctiond/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.junctiond/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.junctiond/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.junctiond/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.junctiond/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/junctiond.service > /dev/null <<EOF
[Unit]
Description=junctiond
After=network-online.target

[Service]
User=$USER
ExecStart=$(which junctiond) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable junctiond
systemctl restart junctiond && journalctl -u junctiond -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
