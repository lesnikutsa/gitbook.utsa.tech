# 💻 Consensus Node

{% hint style="warning" %}
**IMPORTANT** - this guide uses a non-standard directory
{% endhint %}

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev lz4 -y
```

#### Install GO

```shell
ver="1.22.3" && \
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
git clone https://github.com/celestiaorg/celestia-app && cd celestia-app
```

{% hint style="info" %}
As part of the upcoming v3 update called Ginger, you will need to enable bbr on your systems Please note that this setting only applies to the servers you run your validators and full consensus nodes on (celestia-app)

```bash
# first way to check
cd $HOME/celestia-app
make enable-bbr

# alternatively if that doesn't work you can use this command
sudo modprobe tcp_bbr; \
        echo "net.core.default_qdisc=fq" | sudo tee -a /etc/sysctl.conf; \
        echo "net.ipv4.tcp_congestion_control=bbr" | sudo tee -a /etc/sysctl.conf; \
        sudo sysctl -p; \

# to check the functionality, use
sysctl net.ipv4.tcp_congestion_control | awk '{print $3}' 
# bbr
```

Starting from version 3.0.2 there should be the following parameters in config.toml

```bash
recv_rate 10485760
send_rate 10485760
ttl-num-blocks 12
```



```bash
# Example of sending a Signal upgrade transaction from a validator
celestia-appd tx signal signal 3 --from wallet --chain-id celestia --fees 210000utia --home $HOME/.celestia-app-main

# Signal tracking by validators
celestia-appd query signal tally 3 --home $HOME/.celestia-app-main

# View upcoming update height
celestia-appd query signal upgrade --home $HOME/.celestia-app-main
```


{% endhint %}

```bash
git checkout v3.4.0
make build
mv $HOME/celestia-app/build/celestia-appd $HOME/go/bin/celestia-appd

celestia-appd version --long --home $HOME/.celestia-app-main
#version: 3.4.0
#commit: f8c4c743
```



#### We initialize the node to create the necessary configuration files

```shell
celestia-appd init UTSA_guide --chain-id celestia --home $HOME/.celestia-app-main
```

#### Download Genesis

```shell
cd
git clone https://github.com/celestiaorg/networks
cp $HOME/networks/celestia/genesis.json $HOME/.celestia-app-main/config
wget -O $HOME/.celestia-app-main/config/genesis.json "https://share106.00.utsa.tech/celestia-mainnet/genesis.json"

sha256sum ~/.celestia-app-main/config/genesis.json
# 9727aac9bbfb021ce7fc695a92f901986421283a891b89e0af97bc9fad187793
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.celestia-app-main/config/addrbook.json "https://share106.00.utsa.tech/celestia-mainnet/addrbook.json"
```

#### Set up node configuration

```shell
chain_id="celestia"
sed -i -e "s/^chain-id *=.*/chain-id = \"$chain_id\"/" $HOME/.celestia-app-main/config/client.toml

sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.002utia\"/;" ~/.celestia-app-main/config/app.toml

external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.celestia-app-main/config/config.toml

seeds="12ad7c73c7e1f2460941326937a039139aa78884@celestia-mainnet-seed.itrocket.net:40656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.celestia-app-main/config/config.toml

sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.celestia-app-main/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="100"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.celestia-app-main/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.celestia-app-main/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.celestia-app-main/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="kv"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.celestia-app-main/config/config.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/celestia-appd.service > /dev/null <<EOF
[Unit]
Description=celestia-appd mainnet
After=network-online.target

[Service]
User=$USER
ExecStart=$(which celestia-appd) start --home $HOME/.celestia-app-main
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable celestia-appd
systemctl restart celestia-appd && journalctl -u celestia-appd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
