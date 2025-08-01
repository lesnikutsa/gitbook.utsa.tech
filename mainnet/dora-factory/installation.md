# 💻 Installation



{% hint style="warning" %}
This testnet is limited to participants. It is currently not possible for newbies to join. Details about Incentivized Testnet here - https://docs.dorafactory.org/docs/vota-vk
{% endhint %}

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

#### Install GO

```shell
ver="1.21.3"
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
git clone https://github.com/DoraFactory/doravota && cd doravota
git checkout 0.4.3
make install

dorad version --long | grep -e version -e commit -e build
# version: 0.4.3
# commit: 
```

#### We initialize the node to create the necessary configuration files

```shell
dorad init UTSA_guide --chain-id vota-ash
```

#### Download Genesis

```shell
wget -O $HOME/.dora/config/genesis.json "https://raw.githubusercontent.com/DoraFactory/doravota/main/config/mainnet/genesis.json"

# Проверим генезис
sha256sum ~/.dora/config/genesis.json
# bb1f3841ad636a8dc5320ca39266adc00f27c312d0e73eb35f8a4384306bcbd6
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.dora/config/addrbook.json "https://share.utsa.tech/dora/addrbook.json"
```

#### Set up node configuration

```shell
dorad config chain-id vota-ash
dorad config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"10000000000peaka\"/;" ~/.dora/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.dora/config/config.toml
peers="d49defb23a75ff08c924622e3a2f69280983fed7@13.215.46.25:26656,d6e42c70316e354dcb44fd3a86e97b64fa99fc5e@54.179.177.80:26656,220204926f8da0e197572d458e205608306f1ed1@54.251.131.190:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.dora/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.dora/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.dora/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.dora/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.dora/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.dora/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/dorad.service > /dev/null <<EOF
[Unit]
Description=dorad
After=network-online.target

[Service]
User=$USER
ExecStart=$(which dorad) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable dorad
systemctl restart dorad && journalctl -u dorad -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
