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
git clone https://github.com/tellor-io/layer && cd layer
git checkout v3.0.3
make install

layerd version --long | grep -e version -e commit
# version: HEAD
# commit: e5d129c1be6bdddae713f8570e60ea834ea14b42
```

#### We initialize the node to create the necessary configuration files

```shell
layerd init UTSA_guide --chain-id layertest-3
```

#### Download Genesis

```shell
curl https://tellor.test.rpc.nodeshub.online/genesis | jq '.result.genesis' > ~/.layer/config/genesis.json

sha256sum ~/.layer/config/genesis.json
# 1639247cf3c14117dc9cb5b555500fd62fe55fbe9f0410e610f6daef02c53ec9
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.layer/config/addrbook.json "https://share102.utsa.tech/tellor/addrbook.json"
```

#### Set up node configuration

```shell
sed -i.bak -e "s/^chain-id *=.*/chain-id = \"layertest-3\"/;" ~/.layer/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"test\"/;" ~/.layer/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.000025loya\"/;" ~/.layer/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.layer/config/config.toml

peers="59fd40b86c9b65ca717b29ce37b08fdb82c8e61d@3.144.113.220:26656,b26e162aaae52c917f03afd0e09889c1a9f1da13@18.224.20.250:26656,fc1caebd2550a4172bcdc073d0f18e630c44cc26@3.140.238.60:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.layer/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.layer/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.layer/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.layer/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.layer/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.layer/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.layer/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/layerd.service > /dev/null <<EOF
[Unit]
Description=layerd
After=network-online.target

[Service]
User=$USER
ExecStart=$(which layerd) start --home /root/.layer --key-name <name_wallet> --keyring-backend test
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable layerd
systemctl restart layerd && journalctl -u layerd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
