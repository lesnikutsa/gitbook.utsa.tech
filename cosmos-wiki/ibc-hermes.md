# 🌁 IBC (HERMES)

Hermes is a Cosmos internetwork relay built on Rust. In simple words, it is a communication protocol between blockchains. The relay is a central element in the IBC network architecture and provides the extraction and collection of information from networks (blockchains) that cannot directly send messages to each other. Read more about Hermes here - [https://hermes.informal.systems/index.html](https://hermes.informal.systems/index.html)

Hermes is not the only implementation of cosmos relays, for example, there are also ibc-go and ts-relayer, but it is Hermes that has gained popularity and is the choice of most norunners This guide will set up a relayer between Teritori Osmosis and Evmos networks

Any relayer can be installed both on 1 server along with the necessary nodes, and on a separate server, while it is not even necessary to raise your own nodes, but it is enough to know the open RPCs of the worker nodes. But since the success of relaying is highly dependent on latency and I/O rate, it is currently recommended to serve hosts on the same machine as the relaying process.

{% hint style="danger" %}
Since the relay process must be able to request the network back in height for at least 2/3 of the unbonding period (trusting period = 2/3 of the unbonding period), it is recommended to use pruning settings that will keep the full state of the chain for a longer period of time, than unbonding period

**That is, 14 days from 21 or 10 days from 14 or 1.3 days from 2 days**

The required pruning can be calculated using the formula 14 days \* 24 \* 60 \* 60 / average block time

* For 14 days pruning-keep-recent = 200000
* For 10 days pruning-keep-recent = 150000
* For 1.3 days pruning-keep-recent = 18720
* For 1.5 hours pruning-keep-recent = 1000

_P/s - our pruning will be 1000/0/10 (<mark style="color:red;">THIS IS NOT RECOMMENDED</mark>) and we will try to trick the system a bit by changing the config as follows:_

`misbehaviour = false`
{% endhint %}

{% hint style="info" %}
Polkachu for example uses 40000/2000 for the relayers
{% endhint %}

In this example, we will use 1 server, on which we will install both the nodes themselves and Hermes First of all, we need:

* install the two necessary nodes and fully synchronize them. In this example, install Teritori Osmosis and Evmos
* minimum-gas-prices on Osmosis set to 0; on Teritori set to 0.0001
* RPCs must be open and accessible by Hermes (how to do this can be found here)
* indexing should be set to indexer = "kv" (if not changed, then skip this item)
* check the availability of tokens on wallets. It's best to use the same mnemonic on all networks, don't use your relay addresses for anything else, because that could lead to mismatched account sequencing errors

## Hermes installation

**For UBUNTU 22.04 we can download the version**

```bash
mkdir -p $HOME/.hermes/bin

wget https://github.com/informalsystems/hermes/releases/download/v1.5.0/hermes-v1.5.0-x86_64-unknown-linux-gnu.tar.gz
tar -C $HOME/.hermes/bin/ -vxzf hermes-v1.5.0-x86_64-unknown-linux-gnu.tar.gz
echo 'export PATH="$HOME/.hermes/bin:$PATH"' >> $HOME/.bash_profile && \
source $HOME/.bash_profile

# check version
hermes version
# hermes 1.5.0
```

**For UBUNTU 20.04, build from source above # hermes 1.0.0+ed4dd8c**

```bash
# install rust
curl https://sh.rustup.rs -sSf | sh
source "$HOME/.cargo/env"
rustc --version
# rustc 1.70.0 (90c541806 2023-05-31)

cargo version
# cargo 1.70.0 (ec8a8a0ca 2023-04-25)

# install hermes
cargo install ibc-relayer-cli --bin hermes --locked
export PATH="$HOME/.cargo/bin:$PATH"
hermes version
# hermes 1.5.0
```

## Hermes customization

First, let's set the variables. RPC\_ADDR is taken from config.toml - GRPC\_ADDR is taken from app.toml. The value 127.0.0.1 is replaced by the IP address of the server if Hermes is installed separately from the nodes. **The data below is just an example!**

**Chain\_1 (Osmosis)**

```bash
CHAIN_ID_1="osmosis-1"
RPC_ADDR_1="127.0.0.1:46657"
GRPC_ADDR_1="127.0.0.1:9290"
ACCOUNT_PREFIX_1="osmo"
TRUSTING_PERIOD_1="1hours"
DENOM_1="uosmo"
MNEMONIC_1="way festival bargain mass swear flat fish help dinosaur delay lemon dry another fiber belt year smoke glimpse extra fancy acquire method help universe"
MEMO="lesnik_utsa#4480 (UTSA Relayer)"
```

**Chain\_2 (Teritori)**

```bash
CHAIN_ID_2="teritori-1"
RPC_ADDR_2="127.0.0.1:56657"
GRPC_ADDR_2="127.0.0.1:9390"
ACCOUNT_PREFIX_2="tori"
TRUSTING_PERIOD_2="1hours"
DENOM_2="utori"
MNEMONIC_2="way festival bargain mass swear flat fish help dinosaur delay lemon dry another fiber belt year smoke glimpse extra fancy acquire method help universe"
```

**Chain\_3 (Evmos)**

```bash
CHAIN_ID_3="evmos_9001-2"
RPC_ADDR_3="127.0.0.1:60557"
GRPC_ADDR_3="127.0.0.1:9490"
ACCOUNT_PREFIX_3="evmos"
TRUSTING_PERIOD_3="1hours"
DENOM_3="aevmos"
MNEMONIC_3="way festival bargain mass swear flat fish help dinosaur delay lemon dry another fiber belt year smoke glimpse extra fancy acquire method help universe"
```

**Create a config file This file will use the variables specified above**

```bash
tee $HOME/.hermes/config.toml > /dev/null <<EOF
[global]
# укажите уровень детализации вывода журнала ретранслятора Default: 'info'
# допустимые варианты 'error','warn','info','debug','trace'
log_level = 'info'

[mode]

[mode.clients]
enabled = true
refresh = true
misbehaviour = false

[mode.connections]
enabled = false

[mode.channels]
enabled = false

[mode.packets]
enabled = true
clear_interval = 100
clear_on_start = true
tx_confirmation = true

[rest]
# Следует ли включать службу REST или нет. Значение по умолчанию: false
enabled = true
host = '0.0.0.0'
port = 3000

[telemetry]
# Следует ли включать службу телеметрии или нет. Значение по умолчанию: false
enabled = true
host = '0.0.0.0'
port = 3001

[[chains]]
### CHAIN_1 ###
id = '${CHAIN_ID_1}'
rpc_addr = 'http://${RPC_ADDR_1}'
grpc_addr = 'http://${GRPC_ADDR_1}'
websocket_addr = 'ws://${RPC_ADDR_1}/websocket'
rpc_timeout = '10s'
account_prefix = '${ACCOUNT_PREFIX_1}'
key_name = 'wallet'
address_type = { derivation = 'cosmos' }
store_prefix = 'ibc'
max_gas = 15000000
gas_price = { price = 0.0000, denom = '${DENOM_1}' }
gas_multiplier = 1.1
max_msg_num = 15
max_tx_size = 180000
clock_drift = '5s'
max_block_time = '30s'
trusting_period = '${TRUSTING_PERIOD_1}'
trust_threshold = { numerator = '1', denominator = '3' }
memo_prefix = '${MEMO}'
[chains.packet_filter]
policy = 'allow'
list = [
  ['transfer', 'channel-362'], #TERITORI
]

[[chains]]
### CHAIN_2 ###
id = '${CHAIN_ID_2}'
rpc_addr = 'http://${RPC_ADDR_2}'
grpc_addr = 'http://${GRPC_ADDR_2}'
websocket_addr = 'ws://${RPC_ADDR_2}/websocket'
rpc_timeout = '10s'
account_prefix = '${ACCOUNT_PREFIX_2}'
key_name = 'wallet'
address_type = { derivation = 'cosmos' }
store_prefix = 'ibc'
max_gas = 15000000
gas_price = { price = 0.0001, denom = '${DENOM_2}' }
gas_multiplier = 1.1
max_msg_num = 15
max_tx_size = 180000
clock_drift = '5s'
trusting_period = '${TRUSTING_PERIOD_2}'
trust_threshold = { numerator = '1', denominator = '3' }
memo_prefix = '${MEMO}'
[chains.packet_filter]
policy = 'allow'
list = [
  ['transfer', 'channel-0'], #OSMOSIS
  ['transfer', 'channel-1'], #EVMOS
]

[[chains]]
### CHAIN_3 ###
id = '${CHAIN_ID_3}'
rpc_addr = 'http://${RPC_ADDR_3}'
grpc_addr = 'http://${GRPC_ADDR_3}'
websocket_addr = 'ws://${RPC_ADDR_3}/websocket'
rpc_timeout = '10s'
account_prefix = '${ACCOUNT_PREFIX_3}'
key_name = 'wallet'
address_type = { derivation = 'ethermint', proto_type = { pk_type = '/ethermint.crypto.v1.ethsecp256k1.PubKey' } }
store_prefix = 'ibc'
max_gas = 15000000
gas_price = { price = 20000000000, denom = '${DENOM_3}' }
gas_multiplier = 1.1
max_msg_num = 15
max_tx_size = 180000
clock_drift = '5s'
max_block_time = '30s'
trusting_period = '${TRUSTING_PERIOD_3}'
trust_threshold = { numerator = '1', denominator = '3' }
memo_prefix = '${MEMO}'
[chains.packet_filter]
policy = 'allow'
list = [
  ['transfer', 'channel-35'], #TERITORI
]

EOF
```

**Checking if the configuration is correct**

```bash
hermes config validate
# Success: "configuration is valid"

hermes health-check
#  INFO ThreadId(01) using default configuration from '/root/.hermes/config.toml'
#  INFO ThreadId(01) [STRIDE-TESTNET-2] performing health check...
#  INFO ThreadId(01) chain is healthy chain=STRIDE-TESTNET-2
#  INFO ThreadId(01) [GAIA] performing health check...
#  INFO ThreadId(01) chain is healthy chain=GAIA
#  SUCCESS performed health check for all chains in the config
```

**Adding wallets to Hermes**

{% hint style="warning" %}
IMPORTANT - FOR EVM networks add -hd-path "m/44'/60'/0'/0/0"
{% endhint %}

```bash
# create files with mnemonics
tee $HOME/.hermes/${CHAIN_ID_1}.mnemonic > /dev/null <<EOF
${MNEMONIC_1}
EOF
tee $HOME/.hermes/${CHAIN_ID_2}.mnemonic > /dev/null <<EOF
${MNEMONIC_2}
EOF
tee $HOME/.hermes/${CHAIN_ID_3}.mnemonic > /dev/null <<EOF
${MNEMONIC_3}
EOF

# restoring wallets
hermes keys add --chain ${CHAIN_ID_1} --mnemonic-file $HOME/.hermes/${CHAIN_ID_1}.mnemonic
hermes keys add --chain ${CHAIN_ID_2} --mnemonic-file $HOME/.hermes/${CHAIN_ID_2}.mnemonic
hermes keys add --chain ${CHAIN_ID_3} --mnemonic-file $HOME/.hermes/${CHAIN_ID_3}.mnemonic --hd-path "m/44'/60'/0'/0/0"
# Success: Restore key testkey ([ADDRESS]) on [CHAIN ID] chain
# Success: Restore key testkey ([ADDRESS]) on [CHAIN ID] chain
# Success: Restore key testkey ([ADDRESS]) on [CHAIN ID] chain

# see wallets
hermes keys list --chain ${CHAIN_ID_1}
hermes keys list --chain ${CHAIN_ID_2}
hermes keys list --chain ${CHAIN_ID_3}

# view balance
hermes keys balance --chain ${CHAIN_ID_1}
hermes keys balance --chain ${CHAIN_ID_2}
hermes keys balance --chain ${CHAIN_ID_3}
```

**Create a service**

```bash
tee /etc/systemd/system/hermesd.service > /dev/null <<EOF
[Unit]
Description=hermes
After=network-online.target

[Service]
User=$USER
ExecStart=$(which hermes) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```bash
systemctl daemon-reload
systemctl enable hermesd
systemctl restart hermesd && journalctl -u hermesd -f -o cat
```



<pre data-title="# log example" data-line-numbers data-full-width="false"><code><strong>2022-07-31T14:38:30.273575Z  INFO ThreadId(01) Scanned chains:
</strong>2022-07-31T14:38:30.273579Z  INFO ThreadId(01) # Chain: STRIDE-TESTNET-2
  - Client: 07-tendermint-0
    * Connection: connection-0
      | State: OPEN
      | Counterparty state: OPEN
      + Channel: channel-0
        | Port: transfer
        | State: OPEN
        | Counterparty: channel-0
# Chain: GAIA
  - Client: 07-tendermint-0
    * Connection: connection-0
      | State: OPEN
      | Counterparty state: OPEN
      + Channel: channel-0
        | Port: transfer
        | State: OPEN
        | Counterparty: channel-0
2022-07-31T14:38:30.273595Z  INFO ThreadId(01) connection is Open, state on destination chain is Open chain=STRIDE-TESTNET-2 connection=connection-0 counterparty_chain=GAIA
2022-07-31T14:38:30.273600Z  INFO ThreadId(01) connection is already open, not spawning Connection worker chain=STRIDE-TESTNET-2 connection=connection-0
2022-07-31T14:38:30.273606Z  INFO ThreadId(01) no connection workers were spawn chain=STRIDE-TESTNET-2 connection=connection-0
2022-07-31T14:38:30.273611Z  INFO ThreadId(01) channel is OPEN, state on destination chain is OPEN chain=STRIDE-TESTNET-2 counterparty_chain=GAIA channel=channel-0
2022-07-31T14:38:30.276438Z  INFO ThreadId(01) spawned client worker: client::GAIA->STRIDE-TESTNET-2:07-tendermint-0
2022-07-31T14:38:30.278403Z  INFO ThreadId(01) spawned packet worker: packet::channel-0/transfer:STRIDE-TESTNET-2->GAIA
2022-07-31T14:38:30.278430Z  INFO ThreadId(01) done spawning channel workers chain=STRIDE-TESTNET-2 channel=channel-0
2022-07-31T14:38:30.278484Z  INFO ThreadId(01) spawning Wallet worker: wallet::STRIDE-TESTNET-2
2022-07-31T14:38:30.278501Z  INFO ThreadId(01) connection is Open, state on destination chain is Open chain=GAIA connection=connection-0 counterparty_chain=STRIDE-TESTNET-2
2022-07-31T14:38:30.278506Z  INFO ThreadId(01) connection is already open, not spawning Connection worker chain=GAIA connection=connection-0
2022-07-31T14:38:30.278510Z  INFO ThreadId(01) no connection workers were spawn chain=GAIA connection=connection-0
2022-07-31T14:38:30.278513Z  INFO ThreadId(01) channel is OPEN, state on destination chain is OPEN chain=GAIA counterparty_chain=STRIDE-TESTNET-2 channel=channel-0
2022-07-31T14:38:30.283017Z  INFO ThreadId(01) spawned client worker: client::STRIDE-TESTNET-2->GAIA:07-tendermint-0
2022-07-31T14:38:30.286036Z  INFO ThreadId(01) done spawning channel workers chain=GAIA channel=channel-0
2022-07-31T14:38:30.286132Z  INFO ThreadId(01) spawning Wallet worker: wallet::GAIA
2022-07-31T14:38:30.290684Z  INFO ThreadId(01) Hermes has started
</code></pre>

Next, similar logs with transaction hashes should go, and the balance of wallets should begin to decrease. Transactions can also be checked in explorer. If there are such logs, then everything is set up and working.

![](<../.gitbook/assets/image (31).png>)
