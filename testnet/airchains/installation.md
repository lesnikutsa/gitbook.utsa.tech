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
ver="1.21.3" && \
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
mkdir -p $HOME/go/bin

wget https://github.com/airchains-network/junction/releases/download/v0.1.0/junctiond
chmod +x junctiond
mv junctiond $HOME/go/bin/

junctiond version --long | grep -e version -e commit
# version: 
# commit: aab9725fd6fb760a3477229f08394fccbd1e2bbe
```

#### We initialize the node to create the necessary configuration files

```shell
junctiond init UTSA_guide --chain-id junction
```

#### Download Genesis

```shell
wget -O $HOME/.junction/config/genesis.json "https://raw.githubusercontent.com/111STAVR111/props/main/Airchains/genesis.json"

sha256sum ~/.junction/config/genesis.json
# 6a73e3473eb2978b1db0dd572be6316a6fab4321cd5c54e28f1c5733fa9d637c
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.junction/config/addrbook.json "https://share102.utsa.tech/airchains/addrbook.json"
```

#### Set up node configuration

```shell
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.00025amf\"/;" ~/.junction/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.junction/config/config.toml
peers="48887cbb310bb854d7f9da8d5687cbfca02b9968@35.200.245.190:26656,2d1ea4833843cc1433e3c44e69e297f357d2d8bd@5.78.118.106:26656,de2e7251667dee5de5eed98e54a58749fadd23d8@34.22.237.85:26656,1918bd71bc764c71456d10483f754884223959a5@35.240.206.208:26656,ddd9aade8e12d72cc874263c8b854e579903d21c@178.18.240.65:26656,eb62523dfa0f9bd66a9b0c281382702c185ce1ee@38.242.145.138:26656,0305205b9c2c76557381ed71ac23244558a51099@162.55.65.162:26656,086d19f4d7542666c8b0cac703f78d4a8d4ec528@135.148.232.105:26656,3e5f3247d41d2c3ceeef0987f836e9b29068a3e9@168.119.31.198:56256,8b72b2f2e027f8a736e36b2350f6897a5e9bfeaa@131.153.232.69:26656,6a2f6a5cd2050f72704d6a9c8917a5bf0ed63b53@93.115.25.41:26656,e09fa8cc6b06b99d07560b6c33443023e6a3b9c6@65.21.131.187:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.junction/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.junction/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.junction/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.junction/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.junction/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.junction/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/junctiond.service > /dev/null <<EOF
[Unit]
Description=junctiond
After=network-online.target

[Service]
User=$USER
ExecStart=$(which junctiond) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable junctiond
systemctl restart junctiond && journalctl -u junctiond -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
