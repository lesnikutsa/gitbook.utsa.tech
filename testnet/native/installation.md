# 💻 Installation

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

#### Install GO

```shell
ver="1.23.3"
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
git clone https://github.com/gonative-cc/gonative && cd gonative
git checkout v0.1.1
make build
mv $HOME/gonative/out/gonative $HOME/go/bin/gonatived

gonatived version --long | grep -e version -e commit
# version: 0.1.1
# commit: 5bcbb57e4e4689cf515e1c9e6326ae9078f3bdfa
# comet_server_version: v1.0.0-beta.1
# cosmos_sdk_version: v0.52.0-rc.1.0.20250106202622-c6327ea6e403
# runtime_version: v2.0.0-20241219154748-69025c556666
# stf_version: v1.0.0-beta.1
```

#### We initialize the node to create the necessary configuration files

```shell
gonatived init UTSA_guide --chain-id native-t1
```

#### Download Genesis

```shell
#wget -O $HOME/.gonative/config/genesis.json "https://share102.utsa.tech/native/genesis.json"
wget https://github.com/gonative-cc/network-docs/raw/refs/heads/master/genesis/genesis-testnet-1.json.gz
gzip -d genesis-testnet-1.json.gz
mv genesis-testnet-1.json  $HOME/.gonative/config/genesis.json

sha256sum ~/.gonative/config/genesis.json
# 6c7c7ee70b89850dfe9b9c64b24debe67e36ddf5457c8555d4b979146b99e1b0
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.gonative/config/addrbook.json "https://share102.utsa.tech/native/addrbook.json"
```

#### Set up node configuration

```shell
gonatived config set client chain-id native-t1
gonatived config set client keyring-backend os
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.1untiv\"/;" ~/.gonative/config/app.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.gonative/config/config.toml

peers="2e2f0def6453e67a5d5872da7f73002caf55a010@195.3.221.110:52656,612e6279e528c3fadfe0bb9916fd5532bc9be2cd@164.132.247.253:56406,f2e45e48f55f649dee0e8dc4adfa445308ae8018@167.235.247.51:26656,a7577f50cdefd9a7a5e4a673278d9004df9b4bb4@103.219.169.97:56406,236946946eacbf6ab8a6f15c99dac1c80db6f8a5@65.108.203.61:52656,13b49060d7ae1375a04a8eaec8602920f47b5a55@37.142.120.99:27656,48bd9b42755de1a3039089c4cb66b9dd01561001@57.129.53.32:40656,0c2926cdac1e31e39ea137ec3438be6485fd58d7@116.202.85.179:10007,b80d0042f7096759ae6aada870b52edf0dcd74af@65.109.58.158:26056,f0e48f295e0d7c7d03943c82ac01bfc54969320b@78.46.185.199:26656,8eb8024d28b070d21844a90c7c2d34ef4b731365@193.34.213.77:15007,b5f52d67223c875947161ea9b3a95dbec30041cb@116.202.42.156:32107,5be5b41a6aef28a7779002f2af0989c7a7da5cfe@165.154.245.110:26656,b07e0482f43a2add3531e9aa7946609386b2e7e7@65.109.113.219:30656,6edabc54e76245af9f8f739303c788bfb23bd09a@95.216.12.106:24096,ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@176.9.82.221:30656,d0e0d80be68cec942ad46b36419f0ba76d35d134@94.130.138.41:26444,be5b6092815df2e0b2c190b3deef8831159bb9a2@64.225.109.119:26656,49784fe6a1b812fd45f4ac7e5cf953c2a3630cef@136.243.17.170:38656,40cbf43553e7ee3507814e3110a749a7e638aa83@194.163.169.182:24656,1e9a3f3615ec98ea66c41b7e37a724889067bc05@193.24.209.155:12056,7567880ef17ce8488c55c3256c76809b37659cce@161.35.157.54:26656,d856c6c6f195b791c54c18407a8ad4391bd30b99@142.132.156.99:24096,2dacf537748388df80a927f6af6c4b976b7274cb@148.251.44.42:26656,2c1e6b6b54daa7646339fa9abede159519ca7cae@37.252.186.248:26656,fbc51b668eb84ae14d430a3db11a5d90fc30f318@65.108.13.154:52656,48d0fdcc642690ede0ad774f3ba4dce6e549b4db@142.132.215.124:26656,24d51644ea1a6b91bf745f6767a9079d4b35e3d2@37.27.202.188:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.gonative/config/config.toml
seeds="ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@testnet-seeds.polkachu.com:30656"
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.gonative/config/config.toml
```

#### Recommended Database Backend

```bash
sed -i -e "s/^app-db-backend *=.*/app-db-backend = \"goleveldb\"/;" $HOME/.gonative/config/app.toml
sed -i -e "s/^db_backend *=.*/db_backend = \"pebbledb\"/" $HOME/.gonative/config/config.toml
```

```
# app.toml / base configuration options
app-db-backend = "goleveldb"

# config.toml / base configuration options
db_backend = "pebbledb"
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.gonative/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.gonative/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.gonative/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.gonative/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.gonative/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/gonatived.service > /dev/null <<EOF
[Unit]
Description=gonatived
After=network-online.target

[Service]
User=$USER
ExecStart=$(which gonatived) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable gonatived
systemctl restart gonatived && journalctl -u gonatived -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
