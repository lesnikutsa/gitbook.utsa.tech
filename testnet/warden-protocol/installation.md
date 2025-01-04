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
git clone https://github.com/warden-protocol/wardenprotocol && cd wardenprotocol

wget https://github.com/warden-protocol/wardenprotocol/releases/download/v0.5.4/wardend_Linux_x86_64.zip
unzip wardend_Linux_x86_64.zip
rm -rf wardend_Linux_x86_64.zip
chmod +x wardend
mv $HOME/wardenprotocol/wardend $HOME/go/bin

wardend version --long | grep -e version -e commit
# version: 0.5.4
# commit: 9b78f9acf51bf7e511a1dc3920e1ff01a73e61fb
```

#### We initialize the node to create the necessary configuration files

```shell
wardend init UTSA_guide --chain-id chiado_10010-1
```

#### Download Genesis

```shell
wget -O $HOME/.warden/config/genesis.json "https://raw.githubusercontent.com/warden-protocol/networks/main/testnets/chiado/genesis.json"

sha256sum ~/.warden/config/genesis.json
# 8d6bff68b3c709f4d29c1ddae0d4b8394498911efcfdae16350c400c0e54e686
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.warden/config/addrbook.json "https://share.utsa.tech/warden/addrbook.json"
```

#### Set up node configuration

```shell
wardend config set client chain-id chiado_10010-1
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"25000000award\"/;" ~/.warden/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.warden/config/config.toml
peers="b14f35c07c1b2e58c4a1c1727c89a5933739eeea@warden-testnet-peer.itrocket.net:18656,2d2c7af1c2d28408f437aef3d034087f40b85401@52.51.132.79:26656,fcaffd41eb7e3647fa953607449ff5e371c236b8@195.26.245.67:31656,5461e7642520a1f8427ffaa57f9d39cf345fcd47@54.72.190.0:26656,e1ea15d3c460eb9ace279b0b7665015d3c5d2b9e@135.181.210.171:21406"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.warden/config/config.toml
seeds="8288657cb2ba075f600911685670517d18f54f3b@warden-testnet-seed.itrocket.net:18656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.warden/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.warden/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.warden/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.warden/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.warden/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.warden/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/wardend.service > /dev/null <<EOF
[Unit]
Description=wardend
After=network-online.target

[Service]
User=$USER
ExecStart=$(which wardend) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable wardend
systemctl restart wardend && journalctl -u wardend -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

### **Creating a validator**

1. Get your pubkey

```
wardend tendermint show-validator
```

2. Create validator.json

```
nano $HOME/.warden/validator.json
```

3. Insert our config

```
{
  "pubkey": {#pubkey},
  "amount": "1000000000000000000award",
  "moniker": "<MONIKER>",
  "identity": "",
  "website": "",
  "security": "",
  "details": "",
  "commission-rate": "0.05",
  "commission-max-rate": "0.5",
  "commission-max-change-rate": "0.5",
  "min-self-delegation": "1"
}
```

4. Send the transaction

```
wardend tx staking create-validator $HOME/.warden/validator.json \
    --from=<key-name> \
    --chain-id=chiado_10010-1 \
    --fees 250000000000000award -y  --gas auto --gas-adjustment 1.6
```

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
