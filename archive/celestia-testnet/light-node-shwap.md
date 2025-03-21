# 🖥️ Light node (Shwap)

In this guide, we install the Light node on a separate server and use data from Consensus Full Node!

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
ufw allow 2121 comment light_node
```



## Node installation

{% hint style="warning" %}
**IMPORTANT** - starting October 7th, a new version **v0.18.1-mocha** for the Celestia DA layer (Mocha) will be released, which will use [**Shwap**](https://github.com/celestiaorg/CIPs/blob/main/cips/cip-19.md) and will not be compatible with previous versions. For the new release, it will be necessary to remove `Environment=GODEBUG="asynctimerchan=1"` from the service file

The process of setting up and starting a new node remains the same, but the new node will need to synchronize all blocks from the genesis, or use a snapshot synchronized with the new version. Due to optimizations, a significant decrease in hard disk usage is expected

Both protocols will work side by side during a month-long transition period from October 7th to November 7th for Mocha
{% endhint %}

```shell
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node && cd celestia-node
git checkout tags/v0.21.9-mocha
make build
make install

celestia version
#Semantic version: v0.21.9-mocha
#Commit: f9698d454bb76f85d9110d0107016cb00684a9c7
```

```shell
#https://docs.celestia.org/developers/celestia-node-key/
make cel-key
mv $HOME/celestia-node/cel-key /usr/local/bin/

cel-key add light_wallet --keyring-backend test --node.type light --p2p.network mocha
```

```bash
# show wallet address
cel-key list --node.type light --keyring-backend test --p2p.network mocha
```

#### **Initializing the light**

**--core.ip** use the address of our remote RPC node

**--p2p.network** use the chain id of our network

**--core.port** use the gRPC port from our RPC node

**--keyring.keyname** use the name of the wallet we created

```shell
celestia light init \
  --core.ip <RPC_NODE_IP> \
  --p2p.network mocha \
  --core.port 9090 \
  --keyring.keyname light_wallet
```

#### Create a service file

```shell
tee <<EOF >/dev/null /etc/systemd/system/celestia-light.service
[Unit]
Description=celestia-light testnet daemon
After=network-online.target

[Service]
User=$USER
ExecStart=$(which celestia) light start \
  --p2p.network mocha \
  --metrics.tls=true --metrics --metrics.endpoint otel.mocha.celestia.observer \
  --keyring.keyname light_wallet
  
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable celestia-light
systemctl restart celestia-light && journalctl -u celestia-light -f -o cat
```

{% hint style="warning" %}
**Don't forget to save the catalog with keys!!!** `.celestia-light-mocha-4/keys`
{% endhint %}



## Update

```bash
systemctl stop celestia-light
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
celestia light config-update --p2p.network mocha
```

```bash
systemctl restart celestia-light && journalctl -u celestia-light -f -o cat
```



## Node transfer

{% hint style="info" %}
For Light nodes there is no concept of double signature and if suddenly there is a need to transfer node ID to a new server, then it does not matter to us whether the old server is working or not available. The main thing is that we have a copy of two files located at `/root/.celestia-light-mocha-4/keys/`
{% endhint %}

Please note that we do not necessarily need to change the keyring-test wallet Let's look at the most suitable option for saving the Node ID, assuming that the old server is working:

1. Start a new server and fully sync the Light
2. Stop Light on the new server and replace the two files in `/root/.celestia-light-mocha-4/keys/`
3. Be sure to give the necessary rights `chmod 600 /root/.celestia-light-mocha-4/keys/*`
4. Restart Light on the new server and wait for full synchronization
5. Stop the old server



## Useful commands

**Find out Light node id**

```bash
# first, let's generate an authorization token
AUTH_TOKEN=$(celestia light auth admin --p2p.network mocha)
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
celestia p2p info --node.store ~/.celestia-light-mocha-4/
```

**Working with wallets**

```bash
# show wallet address
cel-key list --node.type light --keyring-backend test --p2p.network mocha

# check balance
celestia state balance --node.store ~/.celestia-light-mocha-4/

# restore wallet
cel-key add light_wallet --keyring-backend test --node.type light  --recover --p2p.network mocha
```

**Status**

```bash
celestia header sync-state --node.store  ~/.celestia-light-mocha-4/
```

**Delete**&#x20;

```bash
systemctl stop celestia-light
systemctl disable celestia-light
rm /etc/systemd/system/celestia-light.service
systemctl daemon-reload
cd $HOME && \
rm -rf .celestia-light-mocha-4 .celestia-ap celestia-node && \
rm -rf $(which celestia)
```

