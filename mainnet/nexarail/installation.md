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
git clone https://github.com/Bookings-cpu/nexarail && cd nexarail
#git checkout v0.1.0-rc1-validator-recovery-hotfix
#make build
#cp ./build/nexaraild $HOME/go/bin/

wget https://github.com/Bookings-cpu/nexarail/releases/download/v0.1.0-rc1-validator-recovery-hotfix/nexaraild-linux-amd64
cp $HOME/nexarail/nexaraild-linux-amd64 $HOME/go/bin/nexaraild

nexaraild version
# ABCI: 1.0.0
# BlockProtocol: 11
# P2PProtocol: 8
# Tendermint: 0.37.16

sha256sum $HOME/go/bin/nexaraild
# cdb03d84e2d998e3581f368cc3440fce179c34010398c7343298e94a3d82112c
```

#### We initialize the node to create the necessary configuration files

```shell
nexaraild init UTSA_guide --chain-id nexarail-mainnet-1
```

#### Download Genesis

```shell
wget -O $HOME/.nexarail/config/genesis.json "https://github.com/Bookings-cpu/nexarail/releases/download/mainnet-genesis-nexarail-mainnet-1/genesis.json"

# Проверим генезис
sha256sum ~/.nexarail/config/genesis.json
#f84f5f03d4d54945153c3f68e20e9864fc03c7f35dbeec2b40274f18d152db32
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.nexarail/config/addrbook.json "https://share.utsa.tech/nexarail/addrbook.json"
```

#### Set up node configuration

```shell
sed -i.bak -e "s/^chain-id *=.*/chain-id = \"nexarail-mainnet-1\"/;" ~/.nexarail/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"os\"/;" ~/.nexarail/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025unxrl\"/;" ~/.nexarail/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.nexarail/config/config.toml
peers="96e659f9a87723304dcd614e3ca89d9b6daf26cc@bore.pub:32656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.nexarail/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.nexarail/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="100"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.nexarail/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.nexarail/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.nexarail/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.nexarail/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.nexarail/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/nexaraild.service > /dev/null <<EOF
[Unit]
Description=nexaraild
After=network-online.target

[Service]
User=$USER
ExecStart=$(which nexaraild) start --home /root/.nexarail --minimum-gas-prices 0.025unxrl
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable nexaraild
systemctl restart nexaraild && journalctl -u nexaraild -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
