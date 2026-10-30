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
ver="1.23.12"
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
git clone --branch testnet-2026-10-29 --depth 1 https://github.com/Luggom/Luggom
cd Luggom

go mod download
go mod verify
make install

luggomd version --long
# version: testnet-2026-10-29
# commit: 985e586341cc81ea5d267f0ef8c0d48e43f5204a
```

#### We initialize the node to create the necessary configuration files

```shell
luggomd init UTSA_guide --chain-id luggom-test
```

#### Download Genesis

```shell
wget -O $HOME/.luggom/config/genesis.json "https://luggom.com/downloads/genesis.json"

sha256sum ~/.luggom/config/genesis.json
#27a0c15b71bf2e8c8efa96eaefbf0393eca1a52428ffb18c0306336b1fdece88
```

#### At this stage, we can download the address book

<pre class="language-shell"><code class="lang-shell"><strong>#wget -O $HOME/.luggom/config/addrbook.json "https://share103.utsa.tech/luggom/addrbook.json"
</strong></code></pre>

#### Set up node configuration

```shell
sed -i.bak -e "s/^chain-id *=.*/chain-id = \"luggom-test\"/;" ~/.luggom/config/client.toml
sed -i.bak -e "s/^keyring-backend *=.*/keyring-backend = \"os\"/;" ~/.luggom/config/client.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"1uluggom\"/;" ~/.luggom/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.luggom/config/config.toml
peers="1530baa7b3a07e0cab8dc81c9c08fccc51a47471@62.146.183.122:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.luggom/config/config.toml
seeds="1530baa7b3a07e0cab8dc81c9c08fccc51a47471@62.146.183.122:26656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.luggom/config/config.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.luggom/config/config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="100"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.luggom/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.luggom/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.luggom/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.luggom/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.luggom/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/luggomd.service > /dev/null <<EOF
[Unit]
Description=luggomd
After=network-online.target

[Service]
User=$USER
ExecStart=$(which luggomd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable luggomd
systemctl restart luggomd && journalctl -u luggomd -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
