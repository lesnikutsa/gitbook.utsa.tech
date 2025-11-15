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
ver="1.19.6"
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
git clone https://github.com/aura-nw/aura && cd aura
git checkout v0.9.3
make install
aurad version --long | grep -e version -e commit -e build
# version: v0.9.3
# commit: ae45a4ef71da0e09543a926825662c555af507bd
# build_tags: ""
```



#### We initialize the node to create the necessary configuration files

<pre class="language-shell"><code class="lang-shell"><strong>aurad init UTSA_guide --chain-id aura_6322-2
</strong></code></pre>

#### Download Genesis

```shell
# Download new genesis file
wget https://images.aura.network/aura_6322-2-genesis.tar.gz
tar -xzvf aura_6322-2-genesis.tar.gz
mv aura_6322-2-genesis.json $HOME/.aura/config/genesis.json

# Verify genesis info
cat $HOME/.aura/config/genesis.json | jq '"Genesis Time: " + .genesis_time + " — Chain ID: " + .chain_id'
# this should return
# "Genesis Time: 2024-05-31T13:00:00Z — Chain ID: aura_6322-2"

# Verify sorted shasum
jq -S -c -M '' $HOME/.aura/config/genesis.json | sha256sum
# this should return
# 3ac311ffbbd18edf2b66f35bd8c4be2e9b96913607ccef4d9e7e4b0b574fa24e  -
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.aura/config/addrbook.json "https://share.utsa.tech/aura/addrbook.json"
```

#### Set up node configuration

```shell
aurad config chain-id aura_6322-2
aurad config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025uaura\"/;" ~/.aura/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.aura/config/config.toml
peers="ed15ae05f17dd4e672eec0a96c38364d063b68dc@65.108.6.45:60756"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.aura/config/config.toml
seeds="22a0ca5f64187bb477be1d82166b1e9e184afe50@18.143.52.13:26656,0b8bd8c1b956b441f036e71df3a4d96e85f843b8@13.250.159.219:26656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.aura/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_keep_every="0"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.aura/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.aura/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.aura/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.aura/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null" && \
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.aura/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.aura/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/aurad.service > /dev/null <<EOF
[Unit]
Description=aurad
After=network-online.target

[Service]
User=$USER
ExecStart=$(which aurad) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable aurad
systemctl restart aurad && journalctl -u aurad -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
