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
ver="1.21.3"
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
git clone https://github.com/JackalLabs/canine-chain && cd canine-chain
git checkout v4.3.0-rc.1
make install
```

```shell
canined version --long | grep -e version -e commit
#version: v4.3.0-rc.1
#commit: 2572380db56a116b276e18af131752881ed1243d
```

#### We initialize the node to create the necessary configuration files

```shell
canined init UTSA_guide --chain-id lupulella-2
```

#### Download Genesis

```shell
wget -O $HOME/.canine/config/genesis.json "https://raw.githubusercontent.com/JackalLabs/jackal-chain-assets/main/testnet/genesis.json"

# Проверим генезис
sha256sum ~/.canine/config/genesis.json
# 9701001c2188abf7c117f7192030bcbab358ac1d5b1a61594f443d3b206ab5a2
```

#### At this stage, we can download the address book

<pre class="language-shell"><code class="lang-shell"><strong>
</strong></code></pre>

#### Set up node configuration

```shell
canined config chain-id lupulella-2
canined config keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025ujkl\"/;" ~/.canine/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.canine/config/config.toml
peers="5eedbfbe64b942f4ab54db3842acf3bfab034c24@161.97.74.88:46656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.canine/config/config.toml
seeds="84f520678ef59ea02f942fa6323ec562ca5a3249@45.79.161.178:26656,cecc087977336da1e9ccd2c50097cd9e7d5e1874@141.95.33.39:26656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.canine/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.canine/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.canine/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.canine/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.canine/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.canine/config/app.toml
```

#### Create a service file

```shell
sudo tee /etc/systemd/system/canined.service > /dev/null <<EOF
[Unit]
Description=canined
After=network-online.target

[Service]
User=$USER
ExecStart=$(which canined) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable canined
systemctl restart canined && journalctl -u canined -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
