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
cd
wget -O story-geth https://github.com/piplabs/story-geth/releases/download/v0.11.0/geth-linux-amd64
chmod +x story-geth
mv story-geth $HOME/go/bin/story-geth

story-geth version
# Version: 0.11.0-stable
# Git Commit: f6503708c29cf9b113d710888c51969ca78d79d1
# Git Commit Date: 20241205
```



#### Install story

```bash
cd
git clone https://github.com/piplabs/story && cd story
git checkout v0.13.0
go build -o story ./client
mv $HOME/story/story $HOME/go/bin/

story version
# Version       v0.13.0-stable
# Git Commit    daaa395
```



#### We initialize the node to create the necessary configuration files

```shell
story init --moniker "UTSA_guide" --network odyssey
```

#### Genesis

```shell
# check the genesis
sha256sum ~/.story/story/config/genesis.json
# d332e9082222cc0dd6fe4e9943eafc89b2ce5e118a75ffa01b77e549fdd12587
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.story/story/config/addrbook.json "https://share102.utsa.tech/story/addrbook.json"
```

#### Set up node configuration

```shell
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.story/story/config/config.toml

peers="c5c214377b438742523749bb43a2176ec9ec983c@176.9.54.69:26656,5dec0b793789d85c28b1619bffab30d5668039b7@150.136.113.152:26656,89a07021f98914fbac07aae9fbb12a92c5b6b781@152.53.102.226:26656,443896c7ec4c695234467da5e503c78fcd75c18e@80.241.215.215:26656,2df2b0b66f267939fea7fe098cfee696d6243cec@65.108.193.224:23656,7cc415203fc4c1a6e534e5fed8292467cf14d291@65.21.29.250:3610,fa294c4091379f84d0fc4a27e6163c956fc08e73@65.108.103.184:26656,81eaee3be00b21d0a124016b62fb7176fa05a4f9@185.198.49.133:33556,3508ef280392bd431ea078dec16dcfae89e8eb78@213.239.192.18:26656,b04bae4f88ca12d45fc14be29ce96837b61a72b8@65.109.49.115:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.story/story/config/config.toml
seeds="434af9dae402ab9f1c8a8fc15eae2d68b5be3387@story-testnet-seed.itrocket.net:29656"
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
ExecStart=$HOME/go/bin/story-geth --odyssey --syncmode full --http --http.api eth,net,web3,engine --http.vhosts '*' --http.addr 127.0.0.1 --http.port 8545 --ws --ws.api eth,web3,net,txpool --ws.addr 127.0.0.1 --ws.port 8546
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
