# 💻 Installation

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

### Install GO

```shell
ver="1.22.3" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

### Open the following ports to communicate with other nodes

```bash
ufw allow 26656 comment p2p_port
```

## Node installation

Currently, to run a node, you need to run two separate clients:&#x20;

* `zenrockd`, consensus client
* `zenrock-sidecar,` oracle

```shell
# if necessary, create the go/bin/ directory
mkdir -p $HOME/go/bin/
```

```bash
cd $HOME
mkdir -p zenrock && cd zenrock
```

#### Install zenrock

```bash
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.25.0/zenrockd

chmod +x zenrockd
mv $HOME/zenrock/zenrockd $HOME/go/bin/

zenrockd version --long | grep -e version -e commit
# version: 6.25.0
# commit: a57887ad030eb1d27666d20c70d12c2643b034d4
```

**Initialize the node to create the necessary configuration files**

```bash
zenrockd init UTSA_guide --chain-id diamond-1
```

**Genesis**

```bash
curl -s https://rpc.diamond.zenrocklabs.io/genesis | jq .result.genesis > $HOME/.zrchain/config/genesis.json

# Проверим genesis
sha256sum ~/.zrchain/config/genesis.json
# 1bfacdbf1a80936da968a2b3cc4e38e604c2cd2adae62e83a626ed01dee1080c
```

**Download Addr book**

```bash
wget -O $HOME/.zrchain/config/addrbook.json "https://share.utsa.tech/zenrock/addrbook.json"
```

**Setting up the node configuration**

```shell
zenrockd config set client chain-id diamond-1

sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.01urock"|g' $HOME/.zrchain/config/app.toml

external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.zrchain/config/config.toml

peers="2f037a6461c012f3296ab1815b3c47843bcd7c3a@zenrock-mainnet-peer.itrocket.net:59656,36840303211712d936647da0f74d1498a7e298d1@34.251.37.55:26656,5ad8a5de6318529994da817043b268ef617e37ba@54.216.86.166:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.zrchain/config/config.toml
seeds="e6c3373d68c504bd89bf77c27a8ac30597afeb2d@zenrock-mainnet-seed.itrocket.net:56656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.zrchain/config/config.toml

sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.zrchain/config/config.toml
```

**Create a service file**

```shell
tee /etc/systemd/system/zenrockd.service > /dev/null <<EOF
[Unit]
Description=Zenrock
After=network-online.target

[Service]
User=$USER
ExecStart=$(which zenrockd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable zenrockd
systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```
