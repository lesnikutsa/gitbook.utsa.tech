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
ver="1.24.5" && \
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
git clone https://github.com/atlas-depin/atlas && cd atlas
git checkout v0.1.0
make install

atlasd version --long
#version: HEAD-a0d2fd0a6d4ebdc1136d671ffebc8976a143ab10
#commit: a0d2fd0a6d4ebdc1136d671ffebc8976a143ab10
```

#### We initialize the node to create the necessary configuration files

```shell
atlasd init UTSA_guide --chain-id itineraria-1
```

#### Download Genesis

```shell
wget -O $HOME/.atlas/config/genesis.json "https://raw.githubusercontent.com/atlas-depin/networks/refs/heads/main/testnet/genesis/genesis.json"

sha256sum ~/.atlas/config/genesis.json
#dab3f837420bb77318ff0af382171a32a192c79bc68e5aa8121a1fd29402a297
```

#### At this stage, we can download the address book

```shell
#wget -O $HOME/.atlas/config/addrbook.json "https://share.utsa.tech/atlas/addrbook.json"
```

#### Set up node configuration

```shell
sed -i.bak -e "s/^chain-id *=.*/chain-id = \"itineraria-1\"/;" ~/.atlas/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"os\"/;" ~/.atlas/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025uatl\"/;" ~/.atlas/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.atlas/config/config.toml
peers="0b74c3e67f36bd1a808a5da2cb21f0e831270171@188.68.36.11:28656,c1f90424ad4224b9fd8fd756f602bc5489209db5@65.109.32.61:14356,565f78d4137b3af129cab42e582563ccc83d225f@195.3.222.20:28656,0a6f897a1ba27fd726a8e41074c8a79abb0626ef@95.216.102.220:11556,cb05c18878c91a269dcd656f674fca23ece36837@116.202.218.189:32656,5fdf5cebdca8f41238b9a81a5cc19a1cca0ca566@146.19.24.32:56656,e701138b2a23db46f8fd9518a6cd7669b2f8c679@69.129.192.135:46656,412f338fe2e8e1de27dc33e99a515f4292f837b5@42.119.242.229:26651,9551a410d0038c0f32dadb1d90d76b11d453d75f@65.108.229.19:27016,91afae91a317103d11ba9f876e173a7574ce5fcf@155.94.170.5:26656,875b69641b227f33c8e22cf0396753da46da8ea3@192.3.154.237:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.atlas/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.atlas/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="100"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.atlas/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.atlas/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.atlas/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.atlas/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.atlas/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/atlasd.service > /dev/null <<EOF
[Unit]
Description=atlasd
After=network-online.target

[Service]
User=$USER
ExecStart=$(which atlasd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable atlasd
systemctl restart atlasd && journalctl -u atlasd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
