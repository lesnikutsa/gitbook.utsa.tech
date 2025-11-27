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
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.73.0/zenrockd

chmod +x zenrockd
mv $HOME/zenrock/zenrockd $HOME/go/bin/

zenrockd version --long | grep -e version -e commit
# version: 6.73.0
# commit: 11d7292a3cc6e8d0b6271962d156bfcdc922f04f
```

**Initialize the node to create the necessary configuration files**

```bash
zenrockd init UTSA_guide --chain-id gardia-9
```

**Genesis**

<pre class="language-bash"><code class="lang-bash"><strong>curl -s https://rpc.gardia.zenrocklabs.io/genesis | jq .result.genesis > $HOME/.zrchain/config/genesis.json
</strong>
# check genesis
sha256sum ~/.zrchain/config/genesis.json
# 63e066dac87e6b88f158338e325ad3c352a45e2bc9b96b0b82d3cc35432e4715
</code></pre>

**Download Addr book**

```bash
#wget -O $HOME/.zrchain/config/addrbook.json "https://share106-7.utsa.tech/zenrock/addrbook.json"
```

**Setting up the node configuration**

```shell
zenrockd config set client chain-id gardia-9
#zenrockd config set client keyring-backend test

sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "2.5urock"|g' $HOME/.zrchain/config/app.toml

external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.zrchain/config/config.toml

peers="d76afe7cbe8e6e39f96d67bdf60c7e767893b15e@sentry-1.gardia.zenrocklabs.io:26656,d9927ea0ea2e4bb46d2192f1c94aeb0770ae8f91@sentry-2.gardia.zenrocklabs.io:36656,8f1b7232e8530f7f964ba048270f77e1e90636de@sentry-3.gardia.zenrocklabs.io:46656,9794aa7d5227e05cf997f85bb537ee15f9db425f@sentry-4.gardia.zenrocklabs.io:56656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.zrchain/config/config.toml
seeds=""
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
