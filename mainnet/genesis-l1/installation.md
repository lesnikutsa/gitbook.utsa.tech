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
ver="1.19.1"
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
git clone https://github.com/alpha-omega-labs/genesis-crypto && cd genesis-crypto
git checkout v1.0.0
make install

genesisd version --long
# 1.0.0
# commit: 7cb02ad35442e00b7eb24dc06868577223d48141
```

#### We initialize the node to create the necessary configuration files

<pre class="language-shell"><code class="lang-shell"><strong>genesisd init UTSA_guide --chain-id genesis_29-2
</strong></code></pre>

#### Download Genesis

```shell
wget -O $HOME/.genesisd/config/genesis.json "https://github.com/alpha-omega-labs/genesisd/raw/neolithic/genesis_29-1-state/genesis.json"

# Проверим генезис
sha256sum ~/.genesisd/config/genesis.json
# b8f6939f3bfaeec666c7efb94a683e9295e40705ef08af14d55014e4fa11757f
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.genesisd/config/addrbook.json "https://anode.team/GenesisL1/main/addrbook.json"
```

#### Set up node configuration

```shell
genesisd config chain-id genesis_29-2
genesisd config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025el1\"/;" ~/.genesisd/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.genesisd/config/config.toml
peers=""
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.genesisd/config/config.toml
seeds=""
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.genesisd/config/config.toml
sed -i 's/max_num_inbound_peers =.*/max_num_inbound_peers = 50/g' $HOME/.genesisd/config/config.toml
sed -i 's/max_num_outbound_peers =.*/max_num_outbound_peers = 50/g' $HOME/.genesisd/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.genesisd/config/config.toml
# if necessary, disable JSON RPC Configuration in app.toml 
# nano /root/.genesisd/config/app.toml
# enable = false
# Proposal https://ping.pub/genesisl1/gov/55
sed -i -e "s/^timeout_commit *=.*/timeout_commit = \"10s\"/" $HOME/.genesisd/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_keep_every="0"
pruning_interval="50"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.genesisd/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.genesisd/config/app.toml 
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.genesisd/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.genesisd/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.genesisd/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.genesisd/config/app.toml
```

#### Create a service file

```shell
sudo tee /etc/systemd/system/genesisd.service > /dev/null <<EOF
[Unit]
Description=genesisd
After=network-online.target

[Service]
User=$USER
ExecStart=$(which genesisd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable genesisd
systemctl restart genesisd && journalctl -u genesisd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
