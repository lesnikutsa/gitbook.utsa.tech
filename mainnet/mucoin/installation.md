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
git clone https://github.com/dasgrid/mucoin && cd mucoin
git checkout rewards-v0.8.0
make install

mucoind version --long
# version: rewards-v0.8.0
# commit: 69927b089a03a3a807163412683797b090c48ce7
```

#### We initialize the node to create the necessary configuration files

```shell
mucoind init UTSA_guide --chain-id mucoin-1
```

#### Download Genesis

```shell
wget -O $HOME/.mucoin/config/genesis.json "https://raw.githubusercontent.com/dasgrid/mucoin/main/networks/mucoin-1/genesis.json"

sha256sum ~/.mucoin/config/genesis.json
#87ebb6ceefd4d9a96c23bcefd0481e2c00efb2e2f155ce509944d9d38d9e2e55
```

#### At this stage, we can download the address book

```shell
#wget -O $HOME/.mucoin/config/addrbook.json "https://share.utsa.tech/mucoin/addrbook.json"
```

```
peers="$(curl -sS https://rpc.mucoin.org:443/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}' | sed -z 's|\n|,|g;s|.$||')"
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.mucoin/config/config.toml
```

#### Set up node configuration

<pre class="language-shell"><code class="lang-shell">sed -i.bak -e "s/^chain-id *=.*/chain-id = \"mucoin-1\"/;" ~/.mucoin/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"os\"/;" ~/.mucoin/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025umuc\"/;" ~/.mucoin/config/app.toml

external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.mucoin/config/config.toml

peers="14b942af909ff8740c52bea456949a5de4be98e8@peer-mucoin.vinjan-inc.com:15756,32361fe4a8e26a1096261c031a951ed31bb07598@169.58.22.139:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.mucoin/config/config.toml
#peers="$(curl -sS https://rpc.mucoin.org:443/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}' | sed -z 's|\n|,|g;s|.$||')"
#sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.mucoin/config/config.toml
<strong>
</strong><strong>sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.mucoin/config/config.toml
</strong></code></pre>

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="100"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.mucoin/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.mucoin/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.mucoin/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.mucoin/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.mucoin/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/mucoind.service > /dev/null <<EOF
[Unit]
Description=mucoind
After=network-online.target

[Service]
User=$USER
ExecStart=$(which mucoind) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable mucoind
systemctl restart mucoind && journalctl -u mucoind -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

