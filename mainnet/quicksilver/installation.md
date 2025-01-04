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

#### Setup validator name

```shell
git clone https://github.com/ingenuity-build/quicksilver && cd quicksilver
wget https://github.com/quicksilver-zone/quicksilver/releases/download/v1.7.5/quicksilverd-v1.7.5-amd64
chmod +x quicksilverd-v1.7.5-amd64
mv quicksilverd-v1.7.5-amd64 /root/go/bin/quicksilverd

quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.7.5
#commit: 644d7fd9214c824fb85f37fb9a9bf7d2a4eb5816
```

#### We initialize the node to create the necessary configuration files

```shell
quicksilverd init UTSA_guide --chain-id quicksilver-2
```

#### Download Genesis

```shell
wget -O $HOME/.quicksilverd/config/genesis.json "https://raw.githubusercontent.com/ingenuity-build/mainnet/main/genesis.json"

# Проверим генезис
jq . ~/.quicksilverd/config/genesis.json -S -c | shasum -a256
# cab2352d12f9e388bc633d909a26eaea8fc52904990405cd20d72077415a51d2
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.quicksilverd/config/addrbook.json "https://share.utsa.tech/quicksilver/addrbook.json"
```

#### Set up node configuration

```shell
quicksilverd config chain-id quicksilver-2 
quicksilverd config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025uqck\"/;" ~/.quicksilverd/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.quicksilverd/config/config.toml
seeds="20e1000e88125698264454a884812746c2eb4807@seeds.lavenderfive.com:11156,babc3f3f7804933265ec9c40ad94f4da8e9e0017@seed.rhinostake.com:11156,00f51227c4d5d977ad7174f1c0cea89082016ba2@seed-quick-mainnet.moonshot.army:26650"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.quicksilverd/config/config.toml
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
systemctl restart quicksilverd && sudo journalctl -u quicksilverd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
