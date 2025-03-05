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
ver="1.21.13"
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
git clone https://github.com/xrplevm/node xrpl && cd xrpl
git checkout v6.0.0
make install

exrpd version --long | grep -e version -e commit
# version: v6.0.0
# commit: 6688ca628b4787b41c9f8cfe431dd718753f542b
```

#### We initialize the node to create the necessary configuration files

```shell
exrpd init UTSA_guide --chain-id xrplevm_1449000-1
```

#### Download Genesis

```shell
wget -O $HOME/.exrpd/config/genesis.json "https://raw.githubusercontent.com/xrplevm/networks/refs/heads/main/testnet/genesis.json"

sha256sum ~/.exrpd/config/genesis.json
# 3431b691e99742921d9b092ae7796a791f4a3df0fd532fd3e876245a05e15ea9
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.exrpd/config/addrbook.json "https://share102.utsa.tech/xrpl/addrbook.json"
```

#### Set up node configuration

```shell
sed -i.bak -e "s/^chain-id *=.*/chain-id = \"xrplevm_1449000-1\"/;" ~/.exrpd/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"os\"/;" ~/.exrpd/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025axrp\"/;" ~/.exrpd/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.exrpd/config/config.toml
PEERS=`curl -sL https://raw.githubusercontent.com/xrplevm/networks/main/testnet/peers.txt | sort -R | head -n 10 | awk '{print $1}' | paste -s -d, -`
sed -i.bak -e "s/^seeds *=.*/seeds = \"$PEERS\"/" ~/.exrpd/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.exrpd/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="100"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.exrpd/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.exrpd/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.exrpd/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.exrpd/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.exrpd/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/exrpd.service > /dev/null <<EOF
[Unit]
Description=exrpd
After=network-online.target

[Service]
User=$USER
ExecStart=$(which exrpd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable exrpd
systemctl restart exrpd && journalctl -u exrpd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

### **Creating a validator**

XRPL EVM works using Proof of Authority consensus. In order to start signing new blocks and participating in the network consensus, current validators must accept your node as a new trusted validator. This democratic process requires the approval of a majority of current validators.

To start the process, you need to join discord and select the validator role in the #roles channel. After that, you will need to introduce yourself in the #become-a-validator channel

When filling out the questionnaire, you will need to provide data that identifies your validator

* **Moniker**
* **Validator operator address**

```
exrpd keys show wallet --bech val
```

* Public key

```
exrpd tendermint show-validator
```



{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
