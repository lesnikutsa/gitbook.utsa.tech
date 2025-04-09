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
ver="1.21.13" && \
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
git clone https://github.com/atomone-hub/atomone && cd atomone
git checkout v2.0.0-rc1
make install

atomoned version --long | grep -e version -e commit
# version: v2.0.0-rc1
# commit: aa7aeb7fcbf0c627d53d408ad80562c70c084826
```

#### We initialize the node to create the necessary configuration files

```shell
atomoned init UTSA_guide --chain-id atomone-testnet-1
```

#### Download Genesis

```shell
wget -O $HOME/.atomone/config/genesis.json "https://atomone.fra1.digitaloceanspaces.com/atomone-testnet-1/genesis.json"

sha256sum ~/.atomone/config/genesis.json
# 5bd5d327fd294d8e9653ab97f28703df7d7e59d84238249bdd8dc7ed33408100
```

#### At this stage, we can download the address book

```shell
#wget -O $HOME/.atomone/config/addrbook.json "https://share102.utsa.tech/atomone/addrbook.json"
```

#### Set up node configuration

```
minimum-gas-prices = "0.025uatone,0.025uphoton"
```

```shell
atomoned config chain-id atomone-testnet-1
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.atomone/config/config.toml
peers="5faef8514ed5c460ac2e3bb473f1b7edb4509e56@116.202.156.139:26717,637077d431f618181597706810a65c826524fd74@23.128.116.47:29956,ce191e4f5bbf8a88412b793fbb1e6ff7b0ba1912@134.17.6.22:26657,2231b2285c3ba2f0dec145633d5bc90b8cf782bd@161.97.77.219:26656,39516fa07c501334c9f9d1d97805c6951fe2946b@82.223.197.163:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.atomone/config/config.toml
seeds=""
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.atomone/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.atomone/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="100"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.atomone/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.atomone/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.atomone/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null" && \
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.atomone/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.atomone/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/atomoned.service > /dev/null <<EOF
[Unit]
Description=atomoned
After=network-online.target

[Service]
User=$USER
ExecStart=$(which atomoned) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable atomoned
systemctl restart atomoned && journalctl -u atomoned -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
