# 💻 Installation

{% hint style="info" %}
This guide uses the RocksDB database, which is the default option

In the future, it is recommended to switch to the faster and more efficient ParityDB option. Please note that ParityDB is still experimental and should not be used in production. If you want to test ParityDB, you can add the --database paritydb flag

Switching between backend databases will require resyncing
{% endhint %}

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev libgmp3-dev tar clang bsdmainutils ncdu unzip llvm libudev-dev make protobuf-compiler lz4 -y
```

**Install Docker**

```bash
. <(wget -qO- https://raw.githubusercontent.com/SecorD0/utils/main/installers/docker.sh)
```



## Installing

```shell
# create a catalog
mkdir -p $HOME/.kusama

chown -R $(id -u):$(id -g) $HOME/.kusama

# open the ports used
ufw allow 30333
```

Polkadot/kusama versions - [https://hub.docker.com/r/parity/polkadot/tags](https://hub.docker.com/r/parity/polkadot/tags)

{% hint style="info" %}
Keep in mind that when running polkadot in docker, the process by default listens only on localhost. If you want to connect to your node services (rpc, websockets and prometheus), then you need to make sure you start your node using --rpc-external --ws-external and --prometheus-external
{% endhint %}

{% hint style="info" %}
IMPORTANT - ⚠️ BEEFY is enabled on Westend and Kusama ⚠️ https://github.com/paritytech/polkadot/pull/7661 BEEFY is a consensus protocol that will help with connecting to ethereum or kusama <> polkadot bridging. BEEFY is currently enabled by default and conflicts with sync warp when the --validator flag is enabled Validators using sync warp for Kusama and Westend need to disable BEEFY by adding the --no-beefy flag or remove the --sync=warp flag
{% endhint %}

**Launch docker after specifying the name of the validator**

```bash
docker run -dit \
--name kusama_node \
--restart always \
--network host \
-v $HOME/.kusama:/data -u $(id -u ${USER}):$(id -g ${USER}) \
parity/polkadot:v1.9.0 --base-path /data --chain kusama \
--validator --name "<moniker>" \
--public-addr /ip4/$(wget -qO- eth0.me)/tcp/30333 \
--port 30333 --rpc-port 9933 --prometheus-port 9615 \
--telemetry-url "wss://telemetry-backend.w3f.community/submit/ 1" \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0"
```

Now the node should appear in telemetry





