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
ver="1.24.6" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

## Node installation

#### Setup validator name

```shell
git clone https://github.com/dymensionxyz/dymension && cd dymension
git checkout v4.0.0
make install

dymd version --long | grep -e version -e commit
# version: 5bbee2a0c74474bc159bd28bd2f70782e2352dcd
# commit: v4.0.0
```

#### We initialize the node to create the necessary configuration files

```shell
dymd init UTSA_guide --chain-id dymension_1100-1
```

#### Download Genesis

```shell
wget -O $HOME/.dymension/config/genesis.json "https://github.com/dymensionxyz/networks/raw/main/mainnet/dymension/genesis.json"

# Проверим генезис
sha256sum ~/.dymension/config/genesis.json
# 44a4440d7515cd3b7245bc8ed0ccb1e9ecadd8f24da5508f325f9df0509f916b
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.dymension/config/addrbook.json "https://share.utsa.tech/dymension/addrbook.json"
```

#### Set up node configuration

```shell
dymd config chain-id dymension_1100-1
dymd config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"20000000000adym\"/;" ~/.dymension/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.dymension/config/config.toml
peers="a53c0884c5225f13a99e0af9ef04cd50facee668@84.203.117.234:26691,879aca1f688346ca7a3901aa1e9fc62f48112f01@65.109.124.111:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.dymension/config/config.toml
seeds="284313184f63d9f06b218a67a0e2de126b64258d@seeds.silknodes.io:25155,400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@dymension.rpc.kjnodes.com:14659"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.dymension/config/config.toml

timeout_propose="1.8s"
timeout_commit="500ms"

sed -i "s/^timeout_propose *=.*/timeout_propose = \"$timeout_propose\"/" $HOME/.dymension/config/config.toml
sed -i "s/^timeout_commit *=.*/timeout_commit = \"$timeout_commit\"/" $HOME/.dymension/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.dymension/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.dymension/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.dymension/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.dymension/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.dymension/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/dymd.service > /dev/null <<EOF
[Unit]
Description=dymd
After=network-online.target

[Service]
User=$USER
WorkingDirectory=/root/.dymension
ExecStart=$(which dymd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable dymd
systemctl restart dymd && journalctl -u dymd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
