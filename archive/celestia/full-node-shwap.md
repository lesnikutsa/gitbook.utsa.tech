# 🖥️ Full node (Shwap)

In this guide, we install the Full node on a separate server and use data from Consensus Full Node!

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev lz4 -y
```

#### Install GO

```shell
ver="1.23.1" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

```bash
ufw allow 2121 comment full_node
```

{% hint style="warning" %}
**IMPORTANT - you must use XFS file system. Please do not use ext4, but migrate to XFS/ZFS**
{% endhint %}

## Node installation

```shell
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node && cd celestia-node
git checkout tags/v0.21.9
make build
make install

celestia version
#Semantic version: v0.21.9
#Commit: 3d8fb74bc073fb23303dcfc2804ab85ce316fa24
```

```shell
#https://docs.celestia.org/developers/celestia-node-key/
make cel-key
mv $HOME/celestia-node/cel-key /usr/local/bin/

cel-key add full_wallet --keyring-backend test --node.type full
```

```bash
# show wallet address
cel-key list --node.type full --keyring-backend test
```

#### **Initializing the full**

**--core.ip** use the address of our remote RPC node

**--p2p.network** use the chain id of our network

**--core.port** use the gRPC port from our RPC node

**--keyring.keyname** use the name of the wallet we created

```shell
celestia full init \
  --core.ip <RPC_NODE_IP> \
  --core.port 9090 \
  --keyring.keyname full_wallet
```

#### Create a service file

```shell
tee <<EOF >/dev/null /etc/systemd/system/celestia-full.service
[Unit]
Description=celestia-full mainnet daemon
After=network-online.target

[Service]
User=$USER
ExecStart=$(which celestia) full start --archival \
  --metrics.tls=true --metrics --metrics.endpoint otel.celestia.observer \
  --keyring.keyname full_wallet
  
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable celestia-full
systemctl restart celestia-full && journalctl -u celestia-full -f -o cat
```

{% hint style="warning" %}
**Don't forget to save the catalog with keys!!!** `.celestia-full/keys`
{% endhint %}



## Update

```bash
systemctl stop celestia-full
```

```bash
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node && cd celestia-node
git checkout tags/v0.16.0
make build
make install
make cel-key

celestia version
#Semantic version: v0.16.0
#Commit: 6744f648649ebb5fee1b27faf7aca96ecf4519b2
```

```bash
# update config
celestia full config-update
```

```bash
systemctl restart celestia-full && journalctl -u celestia-full -f -o cat
```



## Node transfer

{% hint style="info" %}
For Full nodes there is no concept of double signature and if suddenly there is a need to transfer node ID to a new server, then it does not matter to us whether the old server is working or not available. The main thing is that we have a copy of two files located at `/root/.celestia-full-mocha-4/keys/`
{% endhint %}

Please note that we do not necessarily need to change the keyring-test wallet Let's look at the most suitable option for saving the Node ID, assuming that the old server is working:

1. Start a new server and fully sync the Full
2. Stop Full on the new server and replace the two files in `/root/.celestia-full-mocha-4/keys/`
3. Be sure to give the necessary rights `chmod 600 /root/.celestia-full-mocha-4/keys/*`
4. Restart Full on the new server and wait for full synchronization
5. Stop the old server



## Useful commands

**Find out Full node id**

```bash
# first, let's generate an authorization token
AUTH_TOKEN=$(celestia full auth admin)
echo $AUTH_TOKEN

# we get the peerId of our node
curl -X POST \
     -H "Authorization: Bearer $AUTH_TOKEN" \
     -H 'Content-Type: application/json' \
     -d '{"jsonrpc":"2.0","id":0,"method":"p2p.Info","params":[]}' \
     http://localhost:26658
```

```bash
#  another way to get Node ID
celestia p2p info --node.store ~/.celestia-full/
```

**Working with wallets**

```bash
# show wallet address
cel-key list --node.type full --keyring-backend test

# check balance
celestia state balance --node.store ~/.celestia-full/

# restore wallet
cel-key add full_wallet --keyring-backend test --node.type full --recover
```

**Status**

```bash
celestia header sync-state --node.store  ~/.celestia-full/
```

**Delete**&#x20;

```bash
systemctl stop celestia-full
systemctl disable celestia-full
rm /etc/systemd/system/celestia-full.service
systemctl daemon-reload
cd $HOME && \
rm -rf .celestia-full .celestia-app celestia-node && \
rm -rf $(which celestia)
```

