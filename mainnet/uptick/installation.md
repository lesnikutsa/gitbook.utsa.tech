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
git clone https://github.com/UptickNetwork/uptick && cd uptick
git checkout v0.3.0
make build -B

mv $HOME/uptick/build/uptickd $HOME/go/bin/

uptickd version --long | grep -e version -e commit -e build_tags
# version: v0.3.0
# commit: c8fec8051a71068bdefa6d6663aace374558eafe
```

#### We initialize the node to create the necessary configuration files

<pre class="language-shell"><code class="lang-shell"><strong>uptickd init UTSA_guide --chain-id uptick_117-1
</strong></code></pre>

#### Download Genesis

```shell
wget -O $HOME/.uptickd/config/genesis.json "https://raw.githubusercontent.com/UptickNetwork/uptick-mainnet/master/uptick_117-1/genesis.json"

# Проверим генезис
sha256sum ~/.uptickd/config/genesis.json
# df80462fed795d877fb1e372175ec66d004056fa0ec98c6c3ed52a6715efc66f
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.uptickd/config/addrbook.json "https://share.utsa.tech/uptick/addrbook.json"
```

#### Set up node configuration

```shell
uptickd config chain-id uptick_117-1
uptickd config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025auptick\"/;" ~/.uptickd/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.uptickd/config/config.toml
peers="170397e75ca2b0f4e9f3b1bb5d0d23f9b10f01c7@uptick-sentry-1.p2p.brocha.in:30597,c0b33353fb70d8d71dcb9c8848b3b4207bd56951@uptick-sentry-2.p2p.brocha.in:30598,23e76540bea9b6851b92e280d7e0c123a0d49521@uptick-sentry-3.p2p.brocha.in:30599,94b63fddfc78230f51aeb7ac34b9fb86bd042a77@uptick-rpc.p2p.brocha.in:30601,f97a75fb69d3a5fe893dca7c8d238ccc0bd66a8f@uptick.seed.brocha.in:30600,48e7e8ca23b636f124e70092f4ba93f98606f604@54.37.129.164:55056,8ecd3260a19d2b112f6a84e0c091640744ec40c5@185.165.241.20:26656,8e924a598a06e29c9f84a0d68b6149f1524c1819@57.128.109.11:26656,f05733da50967e3955e11665b1901d36291dfaee@65.108.195.30:21656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.uptickd/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="nothing"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.uptickd/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.uptickd/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.uptickd/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.uptickd/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.uptickd/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/uptickd.service > /dev/null <<EOF
[Unit]
Description=uptickd
After=network-online.target

[Service]
User=$USER
ExecStart=$(which uptickd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable uptickd
systemctl restart uptickd && journalctl -u uptickd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
