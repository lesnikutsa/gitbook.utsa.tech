# 💻 Installation

{% hint style="warning" %}
Important: Default deployment via docker-compose is extremely insecure, so ensure that only you can access your wallet at http://wallet.localhost/

If you leave ports open, anyone can access your wallet with your IP address. Please create an auth token or close the ports in Docker!
{% endhint %}

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

**Install Docker**

```bash
. <(wget -qO- https://raw.githubusercontent.com/SecorD0/utils/main/installers/docker.sh)
```

## Installing

**Create the necessary directories. We'll use a new directory for each new version**

```shell
mkdir -p ~/.canton
mkdir -p ~/.canton/0.4.19
cd ~/.canton/0.4.19
```

**Download the archive from docker-compose**

```bash
wget https://github.com/digital-asset/decentralized-canton-sync/releases/download/v0.4.19/0.4.19_splice-node.tar.gz
tar xzvf 0.4.19_splice-node.tar.gz
cd ~/.canton/0.4.19/splice-node/docker-compose/validator
```

**We explicitly specify the version for correct assembly**

```bash
export IMAGE_TAG=0.4.19
```

To launch a node, we need to obtain an access token. For Devnet, we can obtain one ourselves; for Testnet and Mainnet, we need to obtain one from a sponsor

Getting a token for DEVNET (valid for 1 hour)

We use it the first time we run it. For subsequent launches, we delete the token and leave the -o ""

```bash
curl -X POST https://sv.sv-1.dev.global.canton.network.sync.global/api/sv/v0/devnet/onboard/validator/prepare
```

**Launching the node**

```bash
./start.sh -s "<SPONSOR_SV_URL>" -o "<ONBOARDING_SECRET>" -p "<party_hint>" -m "<MIGRATION_ID>" -w
```

{% hint style="success" %}
**Where:**

**\<SPONSOR\_SV\_URL>** = https://sv.sv-1.dev.global.canton.network.sync.global

**\<ONBOARDING\_SECRET>** = Your Devnet token \<party\_hint> = Your validator name (example: Val-validator-1)

**\<MIGRATION\_ID>** = 0 for Devnet (see here - https://sync.global/sv-network/)
{% endhint %}

**After a successful launch, you can check the logs**

```bash
cd ~/.canton/0.4.19/splice-node/docker-compose/validator
docker compose logs -f validator
```

<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

Please note that the validator can be stopped with the `./stop.sh` command and restarted with the same start.sh command as above. Subsequent invocations can omit the token and leave the field blank, but the `-o` flag is still required, so you must specify the `-o ""` argument

