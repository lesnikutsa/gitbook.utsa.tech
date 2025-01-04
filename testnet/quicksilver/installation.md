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
git clone https://github.com/ingenuity-build/quicksilver && cd quicksilver
git checkout v1.7.1
make install

quicksilverd version --long | grep -e version -e commit -e build_tags
# version: v1.7.1
# build_tags: netgo ledger muslc,
# commit: f11e862d6c39e8b355c7c4fa55da18ce36357752
```

#### We initialize the node to create the necessary configuration files

```shell
quicksilverd init UTSA_guide --chain-id rhye-3
```

#### Download Genesis

```shell
wget -O $HOME/.quicksilverd/config/genesis.json "https://server-4.itrocket.net/testnet/quicksilver/genesis.json"

# check genesis
sha256sum ~/.quicksilverd/config/genesis.json
# b099fde36685b850e4574ae5df55b601ed7ac5306f36edc152d774ba4c189aae
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.quicksilverd/config/addrbook.json "https://server-4.itrocket.net/testnet/quicksilver/addrbook.json"
```

#### Set up node configuration

```shell
quicksilverd config chain-id rhye-3
quicksilverd config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0001uqck\"/;" ~/.quicksilverd/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.quicksilverd/config/config.toml
peers="2aed12a25bfa92e40ccb95c88692735a9488a17e@quicksilver-testnet-peer.itrocket.net:37656,db9b258c697326e55520211f89b26fe468b0bc84@217.76.62.179:46656,5e83e140ae6a480ec8ac714fb71e0b509227cb9a@185.144.99.18:26656,3d9f459442b0ccfb0f43c6478ffbf5a1ca805dec@74.50.74.186:15651,760a6069c28f0b54548a656518471ca2b60481c6@135.181.133.249:16656,14f759decfc140208c6f438d20eb756519688fea@65.21.136.219:21026,f8b8352a069730141eb1966f5b74193f3faf5911@185.16.39.125:27656,78283975c2bee9b95bbf9408cc974cbab7bfe8ef@65.108.231.124:37656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.quicksilverd/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_keep_every="0"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.quicksilverd/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.quicksilverd/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.quicksilverd/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.quicksilverd/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null" && \
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.quicksilverd/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.quicksilverd/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/quicksilverd.service > /dev/null <<EOF
[Unit]
Description=quicksilver
After=network-online.target

[Service]
User=$USER
ExecStart=$(which quicksilverd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable quicksilverd
systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
