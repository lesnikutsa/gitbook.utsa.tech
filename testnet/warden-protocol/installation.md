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
ver="1.25.1"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```

## Node installation

<pre class="language-shell"><code class="lang-shell">git clone https://github.com/warden-protocol/wardenprotocol &#x26;&#x26; cd wardenprotocol
git checkout v0.7.0-rc3
<strong>apt install -y just
</strong>just wardend

<strong>mv $HOME/wardenprotocol/build/wardend $HOME/go/bin/wardend
</strong>
wardend version --long | grep -e commit -e version
# version: v0.7.0-rc3
# commit: 74225da97ccf237cb917d8bab24f235b8eac5502
</code></pre>

#### We initialize the node to create the necessary configuration files

```shell
wardend init UTSA_guide --chain-id barra_9191-1
```

#### Download Genesis

```shell
wget -O $HOME/.warden/config/genesis.json "https://raw.githubusercontent.com/warden-protocol/networks/main/testnets/barra/genesis.json"

sha256sum ~/.warden/config/genesis.json
#49f3b64f9e6ac92d13d25a38daab311ef65a11a966758e1f08e77e7b07ef2c3b
```

#### At this stage, we can download the address book

```shell
wget -O $HOME/.warden/config/addrbook.json "https://share.utsa.tech/warden/addrbook.json"
```

#### Set up node configuration

```shell
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.warden/config/config.toml

cd $HOME/.warden/config
sed -i.bak 's|^\s*chain-id\s*=.*|chain-id = "barra_9191-1"|' client.toml
sed -i.bak 's|^\s*evm-chain-id\s*=.*|evm-chain-id = 9191|' app.toml
sed -i.bak 's|^\s*minimum-gas-prices\s*=.*|minimum-gas-prices = "10award"|' app.toml
sed -i.bak 's|^\s*seeds\s*=.*|seeds = "c489c003b7c72298840bd4411ffc98ce13e07c27@54.194.136.183:26656,4564c91423a923eaba7982e69e33aec6185d362f@54.72.5.234:26656"|' config.toml
sed -i.bak 's|^\s*timeout_propose\s*=.*|timeout_propose = "1s"|' config.toml
sed -i.bak 's|^\s*timeout_propose_delta\s*=.*|timeout_propose_delta = "200ms"|' config.toml
sed -i.bak 's|^\s*timeout_prevote\s*=.*|timeout_prevote = "500ms"|' config.toml
sed -i.bak 's|^\s*timeout_prevote_delta\s*=.*|timeout_prevote_delta = "200ms"|' config.toml
sed -i.bak 's|^\s*timeout_precommit\s*=.*|timeout_precommit = "500ms"|' config.toml
sed -i.bak 's|^\s*timeout_precommit_delta\s*=.*|timeout_precommit_delta = "200ms"|' config.toml
sed -i.bak 's|^\s*timeout_commit\s*=.*|timeout_commit = "2s"|' config.toml
sed -i.bak 's|^\s*create_empty_blocks\s*=.*|create_empty_blocks = true|' config.toml
```

#### (OPTIONAL) Set up pruning

```shell
pruning="custom"
pruning_keep_recent="100"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.warden/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.warden/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.warden/config/app.toml
```

#### (OPTIONAL) Set up indexer

```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.warden/config/config.toml
```

#### (OPTIONAL) Enable/Disable Snapshots

```shell
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.warden/config/app.toml
```

#### Create a service file

```shell
tee /etc/systemd/system/wardend.service > /dev/null <<EOF
[Unit]
Description=wardend
After=network-online.target

[Service]
User=$USER
ExecStart=$(which wardend) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable wardend
systemctl restart wardend && journalctl -u wardend -f -o cat
```

{% hint style="info" %}
**If peers do not cling for a long time or you see&#x20;**<mark style="color:blue;">**errors error: wrong Block.Header.AppHash**</mark>**, you need to use State sync or boot from a Snapshot**
{% endhint %}

### **Creating a validator**

1. Get your pubkey

```
wardend comet show-validator
```

2. Create validator.json

```
nano $HOME/.warden/validator.json
```

3. Insert our config

```
{
  "pubkey": {#pubkey},
  "amount": "1000000000000000000award",
  "moniker": "<MONIKER>",
  "identity": "",
  "website": "",
  "security": "",
  "details": "",
  "commission-rate": "0.1",
  "commission-max-rate": "0.2",
  "commission-max-change-rate": "0.01",
  "min-self-delegation": "1"
}
```

4. Send the transaction

```
wardend tx staking create-validator $HOME/.warden/validator.json \
    --from=<key-name> \
    --chain-id=barra_9191-1 \
    --fees 2500award -y  --gas auto --gas-adjustment 1.6
```

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
