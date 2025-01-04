# 🛠️ Useful commands

{% hint style="warning" %}
**Transactions and curl commands are launched while the node is running**
{% endhint %}

{% hint style="warning" %}
**If transactions are not sent with an error account sequence mismatch, expected 18, got 17: incorrect account sequence, then add the -s 18 switch to the command (replace the number with the one waiting for sequence)**
{% endhint %}

### First we set the variables

```shell
BINARY="canined"
DENOM="ujkl"
FOLDER=".canine"
```

### **Working with wallets**

<pre class="language-shell"><code class="lang-shell"><strong># create wallet
</strong>$BINARY keys add &#x3C;name_wallet> --keyring-backend os

# restore wallet
$BINARY keys add &#x3C;name_wallet> --recover --keyring-backend os

# restore wallet for EVM networks
$BINARY keys add &#x3C;name_wallet> --recover --coin-type 118 --algo secp256k1

# list wallets
$BINARY keys list

# show account key
$BINARY keys show &#x3C;name_wallet> --bech acc

# show validator key
$BINARY keys show &#x3C;name_wallet> --bech val

# show consensus key
$BINARY keys show &#x3C;name_wallet> --bech cons

# show all supported addresses
$BINARY debug addr &#x3C;wallet_addr>

# how private key
$BINARY keys export &#x3C;name_wallet> --unarmored-hex --unsafe

# account request
$BINARY q auth account $($BINARY keys show &#x3C;name_wallet> -a) -o text

# delete wallet
$BINARY keys delete &#x3C;name_wallet>
</code></pre>

### General information

```shell
# check blocks
$BINARY status 2>&1 | jq ."SyncInfo"."latest_block_height"

# check logs
journalctl -u $BINARY -f -o cat

# check status
curl localhost:26657/status

# check balance
$BINARY q bank balances <address>

# check the pubkey of the validator
$BINARY tendermint show-validator

# check the validator
$BINARY query staking validator <valoper_address>
$BINARY query staking validators --limit 1000000 -o json | jq '.validators[] | select(.description.moniker=="<name_moniker>")' | jq

# check TX_HASH
$BINARY query tx <TX_HASH>

# network settings
$BINARY q staking params
$BINARY q slashing params

# check how many blocks are skipped by the validator
$BINARY q slashing signing-info $($BINARY tendermint show-validator)

# find out the validator creation transaction (replace your valoper_address)
$BINARY query txs --events='create_validator.validator=<your_valoper_address>' -o=json | jq .txs[0].txhash -r

# view active set
$BINARY q staking validators -o json --limit=1000 \
| jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' \
| jq -r '.tokens + " - " + .description.moniker' \
| sort -gr | nl

# viewing an inactive set
$BINARY q staking validators -o json --limit=1000 \
| jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' \
| jq -r '.tokens + " - " + .description.moniker' \
| sort -gr | nl
```

### Transactions

```shell
# collect rewards from all validators delegated to (no validator fee)
$BINARY tx distribution withdraw-all-rewards --from <name_wallet> --fees 5000$DENOM -y

# collect rewards from a separate validator or rewards + commission from your own validator
$BINARY tx distribution withdraw-rewards <valoper_address> --from <name_wallet> --fees 5000$DENOM --commission -y

# delegate more to your stake (this is how 1 coin is sent)
$BINARY tx staking delegate <valoper_address> 1000000$DENOM --from <name_wallet> --fees 5000$DENOM -y

# redelegation to another validator
$BINARY tx staking redelegate <src-validator-addr> <dst-validator-addr> 1000000$DENOM --from <name_wallet> --fees 5000$DENOM -y

# unbond 
$BINARY tx staking unbond <addr_valoper> 1000000$DENOM --from <name_wallet> --fees 5000$DENOM -y

# send coins to another address
$BINARY tx bank send <name_wallet> <address> 1000000$DENOM --fees 5000$DENOM -y

# get out of jail
$BINARY tx slashing unjail --from <name_wallet> --fees 5000$DENOM -y
```

### Governance

```shell
# list proposals
$BINARY q gov proposals

# see the result of the vote
$BINARY q gov proposals --voter <ADDRESS>

# vote for the proposal
$BINARY tx gov vote 1 yes --from <name_wallet> --fees 555$DENOM

# make a deposit to the proposal
$BINARY tx gov deposit 1 1000000$DENOM --from <name_wallet> --fees 555$DENOM

# create an proposal
$BINARY tx gov submit-proposal --title="Randomly reward" --description="Reward 10 testnet participants who completed more than 3 tasks" --type="Text" --deposit="11000000$DENOM" --from=<name_wallet> --fees 500$DENOM
```

### Peers and RPC

<pre class="language-shell"><code class="lang-shell"><strong># find out your peer
</strong>PORTR=$(grep -A 3 "\[p2p\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+") &#x26;&#x26; \
echo $($BINARY tendermint show-node-id)@$(curl ifconfig.me)$PORTR

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

# checking the number of peers
PORT=
curl -s http://localhost:$PORT/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr | split(":")[2])"' | wc -l

# list of monikers of connected peers
curl -s http://localhost:$PORT/net_info | jq '.result.peers[].node_info.moniker'

# Проверка prevotes/precommits
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' &#x26;&#x26; \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'

# check prevote of your validator
curl -s localhost:$PORT/consensus_state -s | grep $(curl -s localhost:26657/status | jq -r .result.validator_info.address[:12])
</code></pre>
