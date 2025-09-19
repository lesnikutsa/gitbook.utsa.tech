---
description: 'Officially stated requirements: 2/4/100 ubuntu 22.04'
---

# 💻 Installation

{% hint style="info" %}
✅**UOMI Finney Incentivized Testnet Launched**

50,000,000 $UOMI tokens allocated to reward node operators starting with this testnet!

Validators are the core of the UOMI ecosystem, powering block production, AI computation, and cross-chain interaction. By running a validator node, you actively contribute to the growth of the network, and $UOMI tokens are rewards for your participation

Choose the node type that suits your setup:

RPC nodes: light, full, or archive, optimized for data access and interaction (these node types are subject to question regarding rewards) Validator nodes: require 48GB of GPU RAM and a full node setup to validate transactions and secure the network
{% endhint %}

{% hint style="warning" %}
**IMPORTANT:**

* The validator requires a server with a video card (cards) that has **48 GB of video memory!!!**&#x20;
* This guide is suitable for ubuntu 24.04
{% endhint %}

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev libclang-dev -y
```

**Ports used**

```bash
AI runs on port 8888

# validator
ufw allow 30333 comment p2p
ufw allow 9944 comment rpc
ufw allow 9615 comment prometheus
```

**Install python 3.10**

```bash
apt install software-properties-common -y
add-apt-repository ppa:deadsnakes/ppa
# enter
apt install python3.10

python3.10 --version
#
```

## AI Installation

**Clone the repository for AI**

```bash
git clone https://github.com/Uomi-network/uomi-node-ai
```

**Create a directory for the validator database**

```bash
mkdir -p $HOME/uomi
```

**Install miniconda**

```bash
mkdir -p /root/miniconda3/
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /root/miniconda3/miniconda.sh
chmod +x /root/miniconda3/miniconda.sh
bash /root/miniconda3/miniconda.sh -b -u -p /root/miniconda3
```

```bash
tmux new-session -s miniconda3
```

<pre class="language-bash"><code class="lang-bash"><strong>source /root/miniconda3/etc/profile.d/conda.sh
</strong>conda create -n uomi-ai python=3.10 -y
conda activate uomi-ai
</code></pre>

**Install the necessary software**

```bash
conda install pytorch==2.2.0 torchvision==0.17.0 torchaudio==2.2.0 pytorch-cuda=12.1 -c pytorch -c nvidia
conda install -c nvidia/label/cuda-12.1.0 cuda-nvcc=12.1 cuda-toolkit=12.1
conda install numpy=1.24
pip install flask
pip install psutil
pip install websocket-client requests
pip install transformers
pip install 'accelerate>=0.26.0'
pip install autoawq
pip install 'triton==3.2.0'
pip install diffusers
```

#### Create a service file

```
nano /etc/systemd/system/uomi-ai.service
```

```shell
[Unit]
Description=UOMI AI API Server
After=network.target

[Service]
User=root
WorkingDirectory=/root/uomi-node-ai
ExecStart=/bin/bash -c "source /root/miniconda3/etc/profile.d/conda.sh && conda activate uomi-ai && python3 uomi-ai.py"
Restart=always
RestartSec=10
TimeoutSec=30
StartLimitIntervalSec=500
StartLimitBurst=5

[Install]
WantedBy=multi-user.target
```

```shell
systemctl daemon-reload
systemctl enable uomi-ai.service
systemctl restart uomi-ai.service && journalctl -u uomi-ai.service -f
```



## Validator setup

**Download genesis**

```bash
wget -O /usr/local/bin/genesis.json "https://github.com/Uomi-network/uomi-node/releases/download/v0.3.0/genesis.json"
chmod +x /usr/local/bin/genesis.json
sha256sum /usr/local/bin/genesis.json
#af55c2c53693a174fc4198bb2e97e1f7aadeabca419040fb75b2d332c1ce6dc3
```

**Download the binary file**

```bash
wget -O /usr/local/bin/uomi "https://github.com/Uomi-network/uomi-node/releases/download/v0.3.2/uomi_ubuntu_24_v0.3.2"
chmod +x /usr/local/bin/uomi
uomi --version
#uomi 0.3.2-21ffa0d0589
```

**Create a wallet and save the output**

```bash
uomi key generate --scheme Sr25519
```

```
# output example
#Secret phrase:     tackle rebuild neither turn degree real capital armed giraffe novel resist human
#Network ID:        substrate
#Secret seed:       0x5c9499827e606bcf284e8a650a96ce13ebf0484bd64a280507ff96d99da6e174
#Public key (hex):  0x14ceec080a6aa464b4235dd951a85669d25e4fa659578f01a7a7dc5c95454931
#Account ID:        0x14ceec080a6aa464b4235dd951a85669d25e4fa659578f01a7a7dc5c95454931
#Public key (SS58): 5CXzHQ3DmYKemRAyY6NPyKsDWiChbeeKtxW3q5RWpmdUQvdR
#SS58 Address:      5CXzHQ3DmYKemRAyY6NPyKsDWiChbeeKtxW3q5RWpmdUQvdR
```

{% hint style="warning" %}
Be sure to save the wallet output. We will need the data later
{% endhint %}

**Generate the Ed25519 key for GRANDPA using your seed phrase obtained above**

```bash
uomi key inspect --scheme Ed25519 "<YOU_SEED>"
```

{% hint style="warning" %}
Be sure to save the wallet output. We will need the data later
{% endhint %}

**Create a service file**

```bash
nano /etc/systemd/system/uomi.service
```

```bash
[Unit]
Description=Uomi Node
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
User=root
Group=root
Restart=always
RestartSec=10
LimitNOFILE=65535
ExecStart=/usr/local/bin/uomi \
    --validator \
    --name "<MONIKER>" \
    --chain "/usr/local/bin/genesis.json" \
    --base-path "/root/uomi" \
    --state-pruning 1000 \
    --blocks-pruning 1000 \
    --enable-evm-rpc \
    --rpc-cors all \
    --prometheus-external \
    --telemetry-url "wss://telemetry.polkadot.io/submit/ 0"

# Hardening
ProtectSystem=strict
PrivateTmp=true
PrivateDevices=true
NoNewPrivileges=true
ReadWritePaths=/root/uomi

[Install]
WantedBy=multi-user.target
```

```bash
systemctl daemon-reload
systemctl enable uomi.service
systemctl restart uomi.service && journalctl -u uomi.service -f
```

{% hint style="warning" %}
IMPORTANT - if the node cannot connect to peers, then add a backup peer to the service file

```
--reserved-nodes "/ip4/5.9.87.231/tcp/30333/p2p/12D3KooWCAChuDHHpNER855r6NpAGDeLrSZ4U3sP6FocSWWJZDf5"
```
{% endhint %}



**Adding Keys STEP 1**

Once your node is up and running, paste your keys

NOTE - replace SECRET\_PHRASE and PUBLIC KEY (hex) with your data obtained with the first command "key generate --scheme Sr25519"

```bash
for key_type in babe imon uomi ipfs; do
    curl -H "Content-Type: application/json" \
        -d '{"id":1, "jsonrpc":"2.0", "method":"author_insertKey", "params":["'$key_type'", "SECRET_PHRASE", "PUBLIC_KEY"]}' \
        http://localhost:9944
done
```

**Adding Keys STEP 2**

NOTE - replace SECRET\_PHRASE and PUBLIC KEY (hex) with your data obtained using the second command "key inspect --scheme Ed25519"

```bash
curl -H "Content-Type: application/json" \
    -d '{"id":1, "jsonrpc":"2.0", "method":"author_insertKey", "params":["gran", "SECRET_PHRASE", "PUBLIC_KEY"]}' \
    http://localhost:9944
```

**Finally after the node is fully synced run the following command to generate session keys when creating a validator in polkadot js**

```bash
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9944
```

