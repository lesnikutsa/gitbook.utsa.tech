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
ver="1.19.6"
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
git clone https://github.com/ojo-network/ojo && cd ojo
git checkout v0.1.2
make install
```

```shell
ojod version --long | grep -e version -e commit -e build
# HEAD-ad5a2377134aa13d7d76575b95613cf8ed12d1e4
# commit: ad5a2377134aa13d7d76575b95613cf8ed12d1e4
# build_tags: netgo ledger,
```

#### We initialize the node to create the necessary configuration files

```shell
ojod init utsa_guide --chain-id ojo-devnet
```

#### Download Genesis

```shell
wget -O $HOME/.ojo/config/genesis.json "https://service.ppnv.space/ojo/genesis.json"

# Проверим генезис
sha256sum ~/.ojo/config/genesis.json
# 6037d1c1a89110c024fc18143eafe33fee19671b9427a4d4ac9c701f7a3c9309
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.ojo/config/addrbook.json "https://service.ppnv.space/ojo/addrbook.json"
```

#### Set up node configuration

```shell
ojod config chain-id ojo-devnet
ojod config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025uojo\"/;" ~/.ojo/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.ojo/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.ojo/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.ojo/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.ojo/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.ojo/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.ojo/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/ojod.service > /dev/null <<EOF
[Unit]
Description=ojod
After=network-online.target

[Service]
User=$USER
ExecStart=$(which ojod) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable ojod
systemctl restart ojod && journalctl -u ojod -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
