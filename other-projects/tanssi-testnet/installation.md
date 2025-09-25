# 💻 Installation

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev libgmp3-dev tar clang bsdmainutils ncdu unzip llvm libudev-dev make protobuf-compiler libclang-dev -y

ufw allow 30335
ufw allow 30333
```

## Installing&#x20;

**Create the necessary directories**

```shell
mkdir -p /root/.tanssi-data
mkdir -p /root/.tanssi-data/bin
mkdir /root/.tanssi-data/dancelight-data
```

**Downloading the binary file**

```bash
wget https://github.com/moondance-labs/tanssi/releases/download/v0.15.0-para/tanssi-node && chmod +x ./tanssi-node
mv ./tanssi-node /root/.tanssi-data/bin

/root/.tanssi-data/bin/tanssi-node --version
#tanssi-node 0.15.0-fd3014ceef7
```

**Downloading the genesis file**

```bash
wget -O /root/.tanssi-data/dancelight-raw-specs.json "https://raw.githubusercontent.com/moondance-labs/tanssi/75e576add204abd321c48cded556c8de14d65618/chains/orchestrator-relays/node/tanssi-relay-service/chain-specs/dancelight-raw-specs.json"
```

**Generate a nodekey and save it**

```bash
/root/.tanssi-data/bin/tanssi-node key generate-node-key --file /root/.tanssi-data/node-key
#
```

#### Create a service file

First, change "NAME-SEQUENCER" and "NAME" to your own values

```
nano /etc/systemd/system/tanssi.service
```

```shell
[Unit]
Description="Dancelight systemd service"
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=on-failure
RestartSec=10
User=root
SyslogIdentifier=tanssi
SyslogFacility=local7
KillSignal=SIGHUP
ExecStart=/root/.tanssi-data/bin/tanssi-node solo-chain \
--name="NAME-SEQUENCER" \
--base-path=/root/.tanssi-data/dancelight-data/container \
--node-key-file=/root/.tanssi-data/node-key \
--telemetry-url='wss://telemetry.polkadot.io/submit/ 0' \
--pool-type=fork-aware \
--database=paritydb \
--rpc-port=9944 \
--prometheus-port=9615 \
--prometheus-external \
--listen-addr=/ip4/0.0.0.0/tcp/30333 \
--state-pruning=2000 \
--blocks-pruning=2000 \
--db-cache=1024 \
--trie-cache-size=1073741824 \
--collator \
--in-peers=13 \
--detailed-log-output \
-- \
--chain=/root/.tanssi-data/dancelight-raw-specs.json \
--name="NAME" \
--sync=fast \
--base-path=/root/.tanssi-data/dancelight-data/relay \
--node-key-file=/root/.tanssi-data/node-key \
--keystore-path=/root/.tanssi-data/dancelight-data/session \
--rpc-methods=unsafe \
--database=paritydb \
--rpc-port=9945 \
--prometheus-port=9616 \
--prometheus-external \
--listen-addr=/ip4/0.0.0.0/tcp/30334 \
--pool-limit=0 \
--db-cache=128 \
--out-peers=15 \
--state-pruning=2000 \
--blocks-pruning=2000 \
--telemetry-url='wss://telemetry.polkadot.io/submit/ 0' \
--bootnodes=/dns4/qco-dancelight-boot-1.rv.dancelight.tanssi.network/tcp/30334/p2p/12D3KooWCekAqk5hv2fZprhqVz8povpUKdJEiHSd3MALVDWNPFzY \
--bootnodes=/dns4/qco-dancelight-rpc-1.rv.dancelight.tanssi.network/tcp/30334/p2p/12D3KooWEwhUb3tVR5VhRBEqyH7S5hMpFoGJ9Anf31hGw7gpqoQY \
--bootnodes=/dns4/ukl-dancelight-rpc-1.rv.dancelight.tanssi.network/tcp/30334/p2p/12D3KooWPbVtdaGhcuDTTQ8giTUtGTEcUVWRg8SDWGdJEeYeyZcT

[Install]
WantedBy=multi-user.target
```

```shell
systemctl daemon-reload
systemctl enable tanssi
systemctl restart tanssi && journalctl -u tanssi -f -o cat
```

{% hint style="warning" %}
Create a wallet or use any existing one - `tanssi-node key generate -w 24`\
You will need to top up this wallet with 10,000 Star
{% endhint %}

Now our node has started to synchronize. We can check our node in [telemetry](https://telemetry.polkadot.io/#list/0x983a1a72503d6cc3636776747ec627172b51272bf45e50a355348facb67a820a)



## Registering keys

[<mark style="color:red;">**https://docs.tanssi.network/node-operators/sequencers/onboarding/account-setup/**</mark>](https://docs.tanssi.network/node-operators/sequencers/onboarding/account-setup/)

