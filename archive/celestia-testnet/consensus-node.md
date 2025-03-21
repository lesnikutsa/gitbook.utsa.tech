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

Starting from version 3.0.0-mocha there should be the following parameters in config.toml

```bash
#config.toml
recv_rate 10485760
send_rate 10485760
ttl-num-blocks 12

#app.toml
max-recv-msg-size 20971520
```

```bash
# Example of sending a Signal upgrade transaction from a validator
celestia-appd-test tx signal signal 3 --from wallet --chain-id mocha-4 --fees 210000utia --home $HOME/.celestia-app-test

# Signal tracking by validators
celestia-appd-test query signal tally 3 --home $HOME/.celestia-app-test

# View upcoming update
celestia-appd-test query signal upgrade --home $HOME/.celestia-app-test
```


{% endhint %}



```bash
git checkout v3.4.2-mocha
make build
mv $HOME/celestia-app/build/celestia-appd $HOME/go/bin/celestia-appd-test

celestia-appd-test version --long --home $HOME/.celestia-app-test
#version: 3.4.2-mocha
#commit: 5b603273
```

#### We initialize the node to create the necessary configuration files

```shell
celestia-appd-test init UTSA_guide --chain-id mocha-4 --home $HOME/.celestia-app-test
```

#### Download Genesis

```shell
cd
git clone https://github.com/celestiaorg/networks
cp $HOME/networks/mocha-4/genesis.json $HOME/.celestia-app-test/config
wget -O $HOME/.celestia-app-test/config/genesis.json "https://share103.utsa.tech/celestia-testnet/genesis.json"

sha256sum ~/.celestia-app-test/config/genesis.json
# 0846b99099271b240b638a94e17a6301423b5e4047f6558df543d6e91db7e575
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.celestia-app-test/config/addrbook.json "https://share103.utsa.tech/celestia-testnet/addrbook.json"
```

#### Set up node configuration

```shell
chain_id="mocha-4"
sed -i -e "s/^chain-id *=.*/chain-id = \"$chain_id\"/" $HOME/.celestia-app-test/config/client.toml

sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.002utia\"/;" ~/.celestia-app-test/config/app.toml

external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.celestia-app-test/config/config.toml

seeds="5d0bf034d6e6a8b5ee31a2f42f753f1107b3a00e@celestia-testnet-seed.itrocket.net:11656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.celestia-app-test/config/config.toml

sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.celestia-app-test/config/config.toml

sed -i -e "s|^target_height_duration *=.*|timeout_commit = \"11s\"|" $HOME/.celestia-app-test/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="100"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.celestia-app-test/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.celestia-app-test/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.celestia-app-test/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="kv"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.celestia-app-test/config/config.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/celestia-appd-test.service > /dev/null <<EOF
[Unit]
Description=celestia-appd-test testnet
After=network-online.target

[Service]
User=$USER
ExecStart=$(which celestia-appd-test) start --home $HOME/.celestia-app-test
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable celestia-appd-test
systemctl restart celestia-appd-test && journalctl -u celestia-appd-test -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
