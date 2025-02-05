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
curl -s https://get.nibiru.fi/@v2.0.0! | bash
mv /usr/local/bin/nibid $HOME/go/bin

nibid version --long | grep -e version -e commit
# version: 2.0.0
# commit: d8a10921ff014d1ecf9e096f2dbdc21d6f521c4b
```

#### We initialize the node to create the necessary configuration files

<pre class="language-shell"><code class="lang-shell"><strong>nibid init UTSA_guide --chain-id cataclysm-1
</strong></code></pre>

#### Download Genesis

```shell
wget -O $HOME/.nibid/config/genesis.json "https://raw.githubusercontent.com/NibiruChain/Networks/main/Mainnet/cataclysm-1/genesis.json"

sha256sum ~/.nibid/config/genesis.json
# 66c3bf943254a7d698849d201e0b7ae1ba7a94118b73f19916727742e26efd99
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.nibid/config/addrbook.json "https://share.utsa.tech/nibiru/addrbook.json"
```

#### Set up node configuration

<pre class="language-shell"><code class="lang-shell"><strong>nibid config chain-id cataclysm-1 
</strong>nibid config keyring-backend os
peers="807df0af03c7de32317eda4fe4dbdcc3ad4b4ae6@208.88.251.53:44441,98cadded622d291141f8a83972fa046267df94b6@38.109.200.36:44441,f0ccacd7cd19f7c30c203ca4c9cbee62d4f8f773@35.234.108.227:26656,8d8324141897243927359345bb4b1bb78a1e1df1@65.109.56.235:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.nibid/config/config.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025unibi\"/;" ~/.nibid/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.nibid/config/config.toml
</code></pre>

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.nibid/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.nibid/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.nibid/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.nibid/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.nibid/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/nibid.service > /dev/null <<EOF
[Unit]
Description=nibid
After=network-online.target

[Service]
User=$USER
ExecStart=$(which nibid) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable nibid
systemctl restart nibid && journalctl -u nibid -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
