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
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v5.5.0/zenrockd
chmod +x zenrockd
mv $HOME/zenrock/zenrockd $HOME/go/bin/

zenrockd version --long | grep -e version -e commit
# version: 5.5.0
# commit: 1c5e92e50435c334cf814377254367392a4dfda5
```

**Initialize the node to create the necessary configuration files**

```bash
zenrockd init UTSA_guide --chain-id gardia-2
```

**Genesis**

```bash
curl -s https://rpc.gardia.zenrocklabs.io/genesis | jq .result.genesis > $HOME/.zrchain/config/genesis.json

# check genesis
sha256sum ~/.zrchain/config/genesis.json
# 85baddb3e1cb6b349aee2d79050b88f781ee34c0d0fc51d9ee2fecd04877ca3c
```

**Download Addr book**

```bash
wget -O $HOME/.zrchain/config/addrbook.json "https://share106-7.utsa.tech/zenrock/addrbook.json"
```

**Setting up the node configuration**

```shell
zenrockd config set client chain-id gardia-2
#zenrockd config set client keyring-backend test

sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0urock"|g' $HOME/.zrchain/config/app.toml

external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.zrchain/config/config.toml

peers="5458b7a316ab673afc34404e2625f73f0376d9e4@zenrock-testnet-peer.itrocket.net:11656,1dfbd854bab6ca95be652e8db078ab7a069eae6f@52.30.152.47:26656,6ef43e8d5be8d0499b6c57eb15d3dd6dee809c1e@52.30.152.47:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.zrchain/config/config.toml
seeds="50ef4dd630025029dde4c8e709878343ba8a27fa@zenrock-testnet-seed.itrocket.net:56656"
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
