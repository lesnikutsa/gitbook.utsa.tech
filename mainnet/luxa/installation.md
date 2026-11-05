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
ver="1.25.0" && \
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
git clone https://github.com/luxaecosystem/luxa-chain && cd luxa-chain
git checkout v1.0.0
make install
luxad version --long

# version: v1.0.0
# commit: ee7cb271c4770fa6435aec937ffe513a082998a7
```

#### We initialize the node to create the necessary configuration files

```shell
luxad init UTSA_guide --chain-id luxa-1
```

#### Download Genesis

```shell
wget -O $HOME/.luxa/config/genesis.json "https://luxaecosystem.xyz/genesis.json"

sha256sum ~/.luxa/config/genesis.json
#3da9b57fa81220b9af6bd78f83956116579305d6474ea08b7756d70167199fa4
```

#### At this stage, we can download the address book

<pre class="language-shell"><code class="lang-shell"><strong>#wget -O $HOME/.luxa/config/addrbook.json "https://share103.utsa.tech/luggom/addrbook.json"
</strong></code></pre>

#### Set up node configuration

```shell
sed -i.bak -e "s/^chain-id *=.*/chain-id = \"luxa-1\"/;" ~/.luxa/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"os\"/;" ~/.luxa/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.001uluxa\"/;" ~/.luxa/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.luxa/config/config.toml
peers="c6eb98f7a65a5fd23ef5c9004d792ec9a8e0b803@51.170.130.74:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.luxa/config/config.toml
seeds=""
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.luxa/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.luxa/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="100"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.luxa/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.luxa/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.luxa/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.luxa/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.luxa/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/luxad.service > /dev/null <<EOF
[Unit]
Description=luxad
After=network-online.target

[Service]
User=$USER
ExecStart=$(which luxad) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable luxad
systemctl restart luxad && journalctl -u luxad -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
