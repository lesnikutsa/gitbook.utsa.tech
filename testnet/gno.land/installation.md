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

```shell
git clone https://github.com/gnolang/gno && cd $HOME/gno
git checkout chain/test13

make -C gno.land install.gnoland install.gnokey
gnoland version
# gnoland version: chain/test13
gnokey --help
```

#### We initialize the node to create the necessary configuration files

```shell
gnoland config init
gnoland secrets init
```

#### Download Genesis

```shell
wget -O $HOME/gno/genesis.json "https://github.com/gnolang/gno/releases/download/chain/test13/genesis.json"

shasum -a 256 $HOME/gno/genesis.json
# 56f56e135174feff9f93283d5ec7e4ec955cd5155108aff5009d4fd51c5adaf2
```

#### Setting up the node configuration

```shell
gnoland config set moniker "<moniker>"
SERVER_IP=$(curl -4 -s ifconfig.me)
gnoland config set p2p.external_address "$SERVER_IP:26656"
gnoland config set p2p.pex true

# persistent peers (required)
gnoland config set p2p.persistent_peers \
"g1e3pftctakyx58mzapqs4syc4h0jcxwtnraq00c@185.248.24.16:47656,g13lg797wyweuultfxdntaz3v9yuchl5p9aexj4k@178.124.192.147:26656,g1z3mavstqrfewltrw886d35fufq6v7eg63xjcet@88.198.46.55:32556,g1j3ylvtrdfzckh5979kvhpznvgy6e0ytwhzrja5@149.50.96.58:41656,g1xjx0hcg67jwqfsmuzjz4uqfhh5ma4rf6n7yg2k@38.246.115.151:26656,g13gumehh3euchv3hp29gsj72pt0ssezaapqu5qd@138.201.240.155:17656,g17su28ydtj8jsdqt2c7m3jn3mysqlz6n57vxd5t@62.210.125.225:26656,g1tsdm60z0akzxtwxkfamt02m6e8fqk0y9h25vks@135.181.78.21:17656,g1xmnn74a2fcc9qnmk6zdlgr5n446em44378c5n3@142.132.158.158:26656,g16ueudzsm5t2v5t2hdr2tr8n2fzj2x6mekx6pvy@192.155.100.132:26656,g1lxkf9gn7kddrr26c640ww5wg3ezsm22we8cjpc@99.81.240.125:26656,g1ghkxmz46w46562yl8capdfqvlypp6zcp55qx7t@185.144.99.19:26656,g1ghl8lhhdnhwwvp94z0gsadvz3c03et30cqyhu2@117.1.104.188:26656,g1wfu406mz57cv2cq68agrkefd4g6pw0ew4kyvlu@65.109.22.211:54656,g1cw4ajr026320c3a6rfm2d77tr94yglh5namap9@149.50.110.40:22656,g17qp5xc8a607svp77h3ttl05mg4fuzyl5fyvc2r@37.60.255.34:27656,g1zz2yvc23ts7uk05gemxmj9dlrhdt7w8pxtcdy5@62.210.207.58:26656,g1pzyfmmtxlla7ay90rfx62cc3rq6hq3jk48cjqz@146.19.24.32:26656,g142k7zc2qym3c0u6jmkf6rv26llgr2f4nakmlmt@54.145.44.95:26656,g1v5unyq7fsndekzegp2xh0tw45nsxpkx5qsgy8u@135.181.232.179:26656,g1dlu8z4kxq8hekht2vmn6z3qqzrea4u37js727a@176.9.3.249:26656,g16f7dlfvrk6qugcdmaq5npw0sjhlf79vrz7ge6c@5.9.8.148:30800,g12gxe0qpq90vhhpp5gtavafgr2nl9cntntvrjkj@186.233.184.95:37656,g1uycj5lkvu97jddywjttd8xq53u3p6eyhh2js25@62.210.124.8:26656"

# consensus settings
gnoland config set application.prune_strategy syncable
gnoland config set consensus.timeout_commit 3s
gnoland config set consensus.peer_gossip_sleep_duration 10ms
gnoland config set p2p.flush_throttle_timeout 10ms

# performance
gnoland config set mempool.size 10000
gnoland config set p2p.max_num_outbound_peers 40
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
  --chainid test-13 \
  --genesis $HOME/gno/genesis.json \
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

Use the faucet https://faucet.gno.land/ and request tokens for your g1xxx address.

Balance Check

```bash
gnokey query --remote "https://rpc.test13.testnets.gno.land" auth/accounts/<ADDRESS>
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
  --gas-wanted 50000000 \
  --chainid test-13 \
  --remote https://rpc.test13.testnets.gno.land \
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

> ℹ️ After a successful transaction you can view your profile at:\
> [https://test13.testnets.gno.land/r/gnops/valopers](https://test13.testnets.gno.land/r/gnops/valopers)

#### Update Description (Optional)

Description limit is **2048 characters**. To update after registration:

```bash
gnokey maketx call \
  --pkgpath gno.land/r/gnops/valopers \
  --func UpdateDescription \
  --args "YOUR-G1-OPERATOR-ADDRESS" \
  --args "YOUR-NEW-DESCRIPTION" \
  --gas-fee 1000000ugnot \
  --gas-wanted 50000000 \
  --chainid test-13 \
  --remote https://rpc.test13.testnets.gno.land \
  --broadcast \
  WALLETNAME
```



{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
