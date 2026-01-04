# 💻 Installation

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

## Node installation

Before installing a node, please read the validator recommendations. It is necessary to configure the validator alongside sentry nodes for security - [https://github.com/network-lumen/validator-kit/blob/master/ops/validator\_specs.md ](https://github.com/network-lumen/validator-kit/blob/master/ops/validator_specs.md)

The team is gradually adding validators to the active set. To make your presence known, set up a validator node that will work through sentry and introduce yourself in the Introduction Discord channel. The team also recommends using servers outside of Europe and Indonesia to increase decentralization and improve your chances of being included in the active set. Learn more about the validator development plans here - [https://github.com/network-lumen/validator-kit/blob/master/ops/stake\_bootstrap.md](https://github.com/network-lumen/validator-kit/blob/master/ops/stake_bootstrap.md)

The project team has created numerous convenient scripts for configuring both the validator and sentry and RPC nodes

**For quick installation, use the script**

```shell
git clone https://github.com/network-lumen/validator-kit.git
cd validator-kit

./join.sh <moniker>

$HOME/validator-kit/bin/lumend version
# v1.3.0

cp $HOME//validator-kit/bin/lumend /usr/local/bin/lumend
lumend version
# v1.3.0

journalctl -u lumend -f -o cat
```

At this point, a full node will be running on the server, and you can leave everything as is or make any necessary changes through the config files

**If the node can't find peers, update the address book**

```shell
wget -O $HOME/.lumen/config/addrbook.json "https://share.utsa.tech/lumen/addrbook.json"
```

{% hint style="warning" %}
Recommendations for validator and sentrys: [https://github.com/network-lumen/validator-kit/blob/master/ops/validator\_specs.md](https://github.com/network-lumen/validator-kit/blob/master/ops/validator_specs.md)

Please note that there are fine-grained settings for sentry and validator. You can use a ready-made script from the team on GitHub or manually change the firewall settings and configuration files.

When manually changing configurations, you must at least:

**Change validator settings**

```bash
!!! CONFIG TOML
seeds = ""
pex = false
persistent_peers = "<PEER_SENTRY1,PEER_SENTRY2>"
private_peer_ids = "<ID_SENTRY1,ID_SENTRY2>"
unconditional_peer_ids = "<ID_SENTRY1,ID_SENTRY2>"
```

**Change settings for sentrys**

```bash
!!! CONFIG TOML
pex = true
persistent_peers = "<PEER_VALIDATOR>"
```

Configure your firewalls to restrict sentry and validator communication

After you've configured everything and launched the nodes, check how many peers your nodes have. The validator value should be equal to the number of your sentries

```bash
# checking the number of peers
PORT=
curl -s http://localhost:$PORT/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr | split(":")[2])"' | wc -l

# list of monikers of connected peers
curl -s http://localhost:$PORT/net_info | jq '.result.peers[].node_info.moniker'
```
{% endhint %}

You can also use snapshot or state-sync. Please use the scripts on GitHub or the commands in the relevant sections of the guide



#### Creating a validator

Create or restore a wallet and save wallet data

```shell
# create a wallet
lumend keys add validator --home ~/.lumen --keyring-backend test

# restore your wallet (after the command, insert the seed)
lumend keys add validator --home ~/.lumen --recover --keyring-backend test
```

To create a validator, you will need to use the script below. This script will:

* Verify that the PQC key validator-pqc exists (and generate one if necessary);
* Link the PQC account to the blockchain, using the existing public consensus key from lumen tendermint show-validator;
* Submit the create-validator staking transaction with minimal self-delegation;
* If necessary, create a structured backup in \~/.lumen/validator-node.bak

```shell
cd
HOME_DIR=~/.lumen FROM=validator \
  ops/scripts/blockchain/become_validator.sh --moniker "<public-validator-name>"
```

{% hint style="warning" %}
**Validators MUST back up BOTH cryptographic keys:**

✅ ed25519 key (classic Cosmos key)\
✅ PQC key (Dilithium)\
👉 If you lose ONE of them, you WILL LOSE ACCESS TO YOUR FUNDS!!!\
Learn more about keys here - [https://github.com/network-lumen/validator-kit/blob/master/learn/validator-key-hardening.md](https://github.com/network-lumen/validator-kit/blob/master/learn/validator-key-hardening.md)
{% endhint %}

You can add delegation later

```shell
HOME_DIR=~/.lumen FROM=validator \
  ops/scripts/blockchain/stake_tokens.sh --amount 1000000ulmn
```

{% hint style="info" %}
To view useful commands, go to [Useful commands](https://utsa.gitbook.io/services/cosmos-wiki/useful-commands)

To create a validator, go to [Creating / Editing a Validator](https://utsa.gitbook.io/services/cosmos-wiki/creating-editing-a-validator)
{% endhint %}
