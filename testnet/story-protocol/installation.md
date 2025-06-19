# 💻 Installation

## Auto Installation <a href="#installation" id="installation"></a>

```bash
wget -O installer_story.sh https://raw.githubusercontent.com/lesnikutsa/story/refs/heads/main/installer_story.sh && chmod +x installer_story.sh && ./installer_story.sh
```



## Manual Installation <a href="#installation" id="installation"></a>

### Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

### Install GO

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

### Open the following ports to communicate with other nodes

```bash
ufw allow 30303 comment story_geth_p2p_port
ufw allow 26656 comment story_p2p_port
```

### Node installation

Currently, to run a node, you need to run two separate clients:&#x20;

* **story-geth**, which is a forked version of geth&#x20;
* **story**, which is a consensus client

```shell
# if necessary, create the go/bin/ directory
mkdir -p $HOME/go/bin/
```



#### Install story-geth

```bash
cd $HOME
rm -rf story-geth
git clone https://github.com/piplabs/story-geth.git
cd story-geth
git checkout v1.1.0
make geth
mv build/bin/geth  $HOME/go/bin/story-geth

story-geth version
# Version: 1.1.0-stable
# Git Commit: fe0d2f85bdc6d8bf39cdfcdb1579c3e10f9a3654
```



#### Install story

```bash
cd
git clone https://github.com/piplabs/story && cd story
git checkout v1.3.0
go build -o story ./client
mv $HOME/story/story $HOME/go/bin/

story version
# Version v1.3.0-stable
# Git Commit 3c01046
```



#### We initialize the node to create the necessary configuration files

```shell
story init --moniker "UTSA_guide" --network aeneid
```

#### Genesis

```shell
# check the genesis
sha256sum ~/.story/story/config/genesis.json
# bf82e0167f262a91b0332a78c04b77fcdcc88aae4fde0816a3341a9a895d0750
```

#### At this stage, we can download the address book

```shell
#wget -O $HOME/.story/story/config/addrbook.json "https://share102.utsa.tech/story/addrbook.json"
```

#### Set up node configuration

```shell
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.story/story/config/config.toml
peers="01f8a2148a94f0267af919d2eab78452c90d9864@story-testnet-peer.itrocket.net:52656,e1623185b6c5403f77533003b0440fae7c33eeed@15.235.224.129:26656,6d77bba865d84eea83f29c48d4bf034ee3540a11@37.27.127.145:26656,803b0100deb519eebaa16b9a55058d21aa8f8dd9@135.181.240.57:33656,311cd3903e25ab85e5a26c44510fbc747ab61760@152.53.87.97:36656,3d7b3efbe94b84112ec4051693438c91890b09fb@144.76.106.228:62656,2440358221774ba82360a08edd4bf5d43ed441a5@65.109.22.211:52656,83b25d26b8b7dd1d4a6f68182b75097d989dcdd0@88.99.137.138:14656,db6791a8e35dee076de75aebae3c89df8bba3374@65.109.50.22:56656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.story/story/config/config.toml
seeds="46b7995b0b77515380000b7601e6fc21f783e16f@story-testnet-seed.itrocket.net:52656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.story/story/config/config.toml

sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.story/story/config/config.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="kv" && \
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.story/story/config/config.toml
```

#### Create a service file story-geth

```shell
tee /etc/systemd/system/story-geth.service > /dev/null <<EOF
[Unit]
Description=Story Geth Client
After=network.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/story-geth --aeneid --syncmode full --http --http.api eth,net,web3,engine --http.vhosts '*' --http.addr 127.0.0.1 --http.port 8545 --ws --ws.api eth,web3,net,txpool --ws.addr 127.0.0.1 --ws.port 8546
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

#### Create a service file story

```shell
tee /etc/systemd/system/story.service > /dev/null <<EOF
[Unit]
Description=Story Consensus Client
After=network.target

[Service]
User=$USER
WorkingDirectory=$HOME/.story/story
ExecStart=$HOME/go/bin/story run
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

#### launch service files

```shell
systemctl daemon-reload
systemctl enable story
systemctl enable story-geth

# story-geth
systemctl restart story-geth && journalctl -u story-geth -f -o cat

# story
systemctl restart story && journalctl -u story -f -o cat
```















{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
