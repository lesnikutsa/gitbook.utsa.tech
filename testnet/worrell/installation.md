# 💻 Installation

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

#### File2Ban

```bash
apt install fail2ban -y && \
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local && \
nano /etc/fail2ban/jail.local
# Uncomment and add your IP: ignoreip = 127.0.0.1/8 ::1 <ip>
systemctl restart fail2ban

systemctl status fail2ban
fail2ban-client status
fail2ban-client status sshd
# logs
tail /var/log/fail2ban.log
```

#### Install GO

```shell
ver="1.24.6"
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
git clone https://github.com/worrellchain/worrell && cd worrell
git checkout v0.1.2
make install

worrelld version --long
# version: v0.1.2
# commit: 2d849e1a33f6204952412a14e7b12cb5ef3870dd
```

#### We initialize the node to create the necessary configuration files

```shell
worrelld init UTSA_guide --chain-id worrell-testnet-1
```

#### Download Genesis

```shell
wget -O $HOME/.worrell/config/genesis.json "https://raw.githubusercontent.com/worrellchain/networks/refs/heads/main/worrell-testnet-1/genesis.json"

sha256sum ~/.worrell/config/genesis.json
#a81c507b12ba0678c3172394ff4bb03e1c3db60050cc5568c127a24ec19378fd
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.worrell/config/addrbook.json "https://share103.utsa.tech/worrell/addrbook.json"
```

#### Set up node configuration

```shell
sed -i.bak -e "s/^chain-id *=.*/chain-id = \"worrell-testnet-1\"/;" ~/.worrell/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"os\"/;" ~/.worrell/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025uworrell\"/;" ~/.worrell/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.worrell/config/config.toml
peers="bb9164c1bd9ed9ff2c0fd9e09b23285698e231de@164.68.98.186:26656,40128ea31b1cfb5d4b24fc9e32ee0c468586c983@worrell-testnet-peer.itrocket.net:12656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.worrell/config/config.toml
seeds=""
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.worrell/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.worrell/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="100"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.worrell/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.worrell/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.worrell/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.worrell/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.worrell/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/worrelld.service > /dev/null <<EOF
[Unit]
Description=worrelld
After=network-online.target

[Service]
User=$USER
ExecStart=$(which worrelld) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable worrelld
systemctl restart worrelld && journalctl -u worrelld -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
