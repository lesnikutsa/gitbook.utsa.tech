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
ver="1.23.3"
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
git clone https://github.com/Bookings-cpu/nexarail && cd nexarail
wget -O $HOME/go/bin/nexaraild "https://github.com/Bookings-cpu/nexarail/releases/download/v0.1.1-mainnet2-fundsafety/nexaraild-linux-amd64"
chmod +x $HOME/go/bin/nexaraild

nexaraild version
# ABCI: 1.0.0

sha256sum $HOME/go/bin/nexaraild
# 068aee2853e452a055de2b4082259ff4ccf42573362310f609f22940399b26ca
```

#### We initialize the node to create the necessary configuration files

```shell
nexaraild init UTSA_guide --chain-id nexarail-mainnet-2
```

#### Download Genesis

```shell
wget -O $HOME/.nexarail/config/genesis.json "https://github.com/Bookings-cpu/nexarail/releases/download/mainnet-genesis-nexarail-mainnet-2/genesis.json"

# Проверим генезис
sha256sum ~/.nexarail/config/genesis.json
#d2d6933fbdf2fed1727c9906dfb41024871c3c20636a6b366ae8414d5af54d62
```

#### At this stage, we can download the address book

```shell
#wget -O $HOME/.nexarail/config/addrbook.json "https://share.utsa.tech/nexarail/addrbook.json"
```

#### Set up node configuration

```shell
sed -i.bak -e "s/^chain-id *=.*/chain-id = \"nexarail-mainnet-2\"/;" ~/.nexarail/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"os\"/;" ~/.nexarail/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025unxrl\"/;" ~/.nexarail/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.nexarail/config/config.toml
peers="1af9139d59677cc42ff2924c89f9cf9e0c980246@5.161.85.160:26656,d91372d5918d870c7861a2eb3e99b90d7c5882ca@5.161.103.203:26656,8776e496483fefbbc4dc17749f5ec80ab121fe2c@5.161.99.76:26656,45668d1ae375f39fce47dfa96e76db7946da7188@5.161.94.47:26656,bbd2b07264da053541128a0677540bc95bf415c6@5.161.72.48:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.nexarail/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.nexarail/config/config.toml

sed -i 's/^timeout_commit = .*/timeout_commit = "1400ms"/' $HOME/.nexarail/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="100"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.nexarail/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.nexarail/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.nexarail/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.nexarail/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.nexarail/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/nexaraild.service > /dev/null <<EOF
[Unit]
Description=nexaraild
After=network-online.target

[Service]
User=$USER
ExecStart=$(which nexaraild) start --home /root/.nexarail --minimum-gas-prices 0.025unxrl
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable nexaraild
systemctl restart nexaraild && journalctl -u nexaraild -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
