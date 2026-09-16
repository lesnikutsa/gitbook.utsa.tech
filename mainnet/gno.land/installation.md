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
ver="1.20.3"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```

## Node installation

```bash
git clone https://github.com/gnolang/gno && cd $HOME/gno
git checkout 31b6650a100d9baf14e7669f8f0df924f1f841e0

make -C gno.land install.gnoland install.gnokey
gnoland version
# gnoland version: HEAD.3426+31b6650a1
gnokey --help
```

#### We initialize the node to create the necessary configuration files

```shell
gnoland config init
gnoland secrets init
```

#### Download Genesis

```shell
wget -O $HOME/gno/gnoland-data/config/genesis.json "https://github.com/gnolang/gno/releases/download/chain/mainnet/genesis.json"

shasum -a 256 $HOME/gno/gnoland-data/config/genesis.json
# ea22691003130eae3ba975b7d16460706b5d75ce6c04ae82c0c4faeab7de91f0
```

#### Setting up the node configuration

```shell
gnoland config set moniker "<moniker>"
SERVER_IP=$(curl -4 -s ifconfig.me)
gnoland config set p2p.external_address "$SERVER_IP:26656"
gnoland config set p2p.pex true

# persistent peers (required)
# gnoland config set p2p.seeds g1ypcgv5auk2rsgltjmcc6wa7m3kaudp6ag2v4n7@seed.gnoland-1.gno.aviaone.com:10299
gnoland config set p2p.persistent_peers "g15rcv5yqef3kvnmueqvkyw8y05sd40jz9p3n5su@seed-1.gno.land:26656,g1ck2yeyvvnpl92237gcea0z68jx07a4nnyvuaan@seed-2.gno.land:26656"

# consensus settings
gnoland config set application.prune_strategy syncable
gnoland config set consensus.timeout_commit 3s
gnoland config set consensus.peer_gossip_sleep_duration 10ms
gnoland config set p2p.flush_throttle_timeout 10ms

# performance
gnoland config set mempool.size 10000
```

#### Create a service file

```shell
tee /etc/systemd/system/gnoland.service > /dev/null <<EOF
[Unit]
Description=Gnoland
After=network-online.target
Wants=network-online.target

[Service]
User=$USER
WorkingDirectory=$HOME/gno
Environment=GNOROOT=$HOME/gno
Environment=HOME=$HOME
ExecStart=$(which gnoland) start \
  --chainid gnoland-1 \
  --genesis $HOME/gno/gnoland-data/config/genesis.json \
  --log-level info \
  --skip-genesis-sig-verification
Restart=on-failure
RestartSec=5s
LimitNOFILE=65535
StandardOutput=journal
StandardError=journal
SyslogIdentifier=gnoland

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable gnoland
systemctl restart gnoland && journalctl -u gnoland -f -o cat
```

#### Create or restore a wallet and save the withdrawal

```bash
# create a wallet. The wallet will be created at $HOME/.config/gno/data/keys.db/
gnokey add <name_wallet>

# restore wallet
gnokey add <name_wallet> --recover

# get your operator's address:
gnokey list
```

{% hint style="warning" %}
Don't forget to save the seed!!!
{% endhint %}

#### **Faucet**

Use the faucet and request tokens for your g1xxx address.

Balance Check

```bash
gnokey query --remote "https://rpc.gno.land" auth/accounts/<ADDRESS>
#gnokey query --remote "http://127.0.0.1:26657" auth/accounts/<ADDRESS>
```

#### **Register as a validator candidate**

{% hint style="info" %}
⚠️ Gnoland uses a validator registration system based on GovDAO. Registration only makes you a candidate. A GovDAO member must create and submit a proposal to include you as an active validator
{% endhint %}

Get your Validator's public key (gpub1...)

```bash
cd /root/gno && gnoland secrets get validator_key
#{
#  "address": "g1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
#  "pub_key": "gpub1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
#}
```

⚠️ In test13, only the `pub_key` is used for registration. The `address` here is the consensus key address — **do not use it** as the operator address. Use your wallet address from `gnokey list` instead.

**Submit Validator Registration**

```bash
gnokey maketx call \
  --pkgpath gno.land/r/gnops/valopers \
  --func Register \
  --args "MONIKER" \
  --args "DESCRIPTION" \
  --args "data-center" \
  --args "OPERATOR_ADDRESS" \
  --args "VAL_PUBKEY" \
  --gas-fee 1000000ugnot \
  --gas-wanted 60000000 \
  --chainid sapphire-1 \
  --remote https://rpc.gno.land \
  --broadcast \
  WALLETNAME
```

| Placeholder        | Description                                                        |
| ------------------ | ------------------------------------------------------------------ |
| `MONIKER`          | Your validator display name                                        |
| `DESCRIPTION`      | Short description of your validator                                |
| `data-center`      | Your infrastructure location                                       |
| `OPERATOR_ADDRESS` | Your **wallet** `g1...` address from `gnokey list`                 |
| `VAL_PUBKEY`       | `pub_key` from `cd /root/gno && gnoland secrets get validator_key` |
| `WALLETNAME`       | Key name from `gnokey list`                                        |

> ℹ️ After a successful transaction you can view your profile at: [https://sapphire.testnets.gno.land/r/gnops/valopers](https://sapphire.testnets.gno.land/r/gnops/valopers)<br>

#### Update Description (Optional)

Description limit is **2048 characters**. To update after registration:

```bash
gnokey maketx call \
  --pkgpath gno.land/r/gnops/valopers \
  --func UpdateDescription \
  --args "YOUR-G1-OPERATOR-ADDRESS" \
  --args "YOUR-NEW-DESCRIPTION" \
  --gas-fee 1000000ugnot \
  --gas-wanted 60000000 \
  --chainid gnoland-1 \
  --remote https://rpc.gno.land \
  --broadcast \
  WALLETNAME
```



{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
