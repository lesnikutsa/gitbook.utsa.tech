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
ver="1.24.5"
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
git clone https://github.com/Limonata-Blockchain/limonata.git && cd limonata
git checkout limonata-v0.3.1
make install
mv $HOME/go/bin/evmd $HOME/go/bin/limonatad

limonatad version --long
# version: limonata-v0.3.1
# commit: 1f7382ef8616e9b69fb53b321fdbb94250c8425b
```

```shell
limonatad init UTSA_guide --chain-id limonata_10777-1
```

#### Download Genesis

```shell
curl -s https://limonata.xyz/genesis.json -o $HOME/.evmd/config/genesis.json

sha256sum ~/.evmd/config/genesis.json
#bb76a6d8abbb1bdeaa41d92811066b16bd6a48f58f7136e3fcb71226b6af4569
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.evmd/config/addrbook.json "https://share101.utsa.tech/limonata/addrbook.json"
```

#### Set up node configuration

```shell
sed -i.bak -e "s/^chain-id *=.*/chain-id = \"limonata_10777-1\"/;" ~/.evmd/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"os\"/;" ~/.evmd/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0aLIMO\"/;" ~/.evmd/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.evmd/config/config.toml
peers="14673dfbd8efff7eed0a97880efde1d0a54da948@195.201.160.23:19156"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.evmd/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.evmd/config/config.toml
sed -i -E "s|type = \".*\"|type = \"app\"|g" $HOME/.evmd/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.evmd/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.evmd/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.evmd/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.evmd/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.evmd/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/limonatad.service > /dev/null <<EOF
[Unit]
Description=limonatad
After=network-online.target

[Service]
User=$USER
ExecStart=$(which limonatad) start --chain-id limonata_10777-1 --evm.evm-chain-id 10777 --minimum-gas-prices 0aLIMO
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable limonatad
systemctl restart limonatad && journalctl -u limonatad -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
