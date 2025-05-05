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
git checkout v1.0.13
make install

arkeod version --long | grep -e version -e commit
# version: v1.0.13
# commit: 452e2688ae9fb343daabeb4986f220164f0f5401
```

#### We initialize the node to create the necessary configuration files

```shell
arkeod init UTSA_guide --chain-id arkeo-main-v1
```

#### Download Genesis

```shell
wget https://github.com/arkeonetwork/arkeo/raw/refs/heads/master/networks/mainnet/arkeo-main-v1/genesis.mainnet.json.gz
gzip -d genesis.mainnet.json.gz
mv genesis.mainnet.json  $HOME/.arkeo/config/genesis.json

sha256sum ~/.arkeo/config/genesis.json
# 2746207cb2e0e7006ec7ac03f41418d81a3b79108f89578bd550ad1ab3d176ab
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.arkeo/config/addrbook.json "https://share102.utsa.tech/arkeo/addrbook.json"
```

#### Set up node configuration

```shell
sed -i.bak -e "s/^chain-id *=.*/chain-id = \"arkeo-main-v1\"/;" ~/.arkeo/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"os\"/;" ~/.arkeo/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.001uarkeo\"/;" ~/.arkeo/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.arkeo/config/config.toml
peers="e21ebcb0b2694e7b316f2f8de883300cffc93b32@peer1.arkeo.network:26656,b8653eecacbe3f413046beb0e8b53d8f520c925e@peer2.arkeo.network:26656, f3037b238720c022be888890b7d8ecb516ae2a05@peer3.arkeo.network:26656,2e0a5e51ae1eabf527eb54632feb6a90ae0704ba@204.16.245.181:26656,4b60b22753c88f3cd6ba42dae8170e1a22429e76@141.95.3.94:26656, 56a47e4ad462edfba90d0409785bc56196fdf376@51.89.98.102:55926,57c53dc1149c8696c839fc5a230579327d650e4c@65.109.114.178:26656,637609e9fe4618fe1d5c7c3564dc9ce4678abf61@142.132.251.87:15856,beaf7267d852cfbaaaaccbc1f92e785e5e0f0420@18.218.164.255:26656,de8f228211e72e8bb206e4f0f5e6e703cb2505eb@95.217.36.103:26656,e077b7ffdfcd6ea6826a126d0003a98fe0218bf7@213.239.194.132:15856,e87f1d4cfa4b7c70defa93dffefc450e2a1c1dc4@44.240.61.167:26656,a3998b8a50765975be2be59954db0f6de66f92e3@5.161.246.27:36657"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.arkeo/config/config.toml
seeds="4d2c67a1d732679826b2f71c833e94b3718c2b50@seed2.arkeo.network:26656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.arkeo/config/config.toml
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
