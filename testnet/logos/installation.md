# 💻 Installation

## Server preparation

```shell
apt update && apt upgrade -y
```

```shell
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

#### Install rust

```shell
curl https://sh.rustup.rs -sSf | sh
source $HOME/.cargo/env

rustup --version
rustc --version
cargo --version
```

## Node installation

Download the diagrams

By default, the node looks for circuit files in `~/.logos-blockchain-circuits`

```shell
wget https://github.com/logos-blockchain/logos-blockchain/releases/download/0.1.2/logos-blockchain-circuits-v0.4.2-linux-x86_64.tar.gz
tar -xzf logos-blockchain-circuits-v0.4.2-linux-x86_64.tar.gz
rm logos-blockchain-circuits-v0.4.2-linux-x86_64.tar.gz

mv logos-blockchain-circuits-*/ ~/.logos-blockchain-circuits
```

#### Installing Ubuntu 22.04 binaries

```shell
cd
mkdir -p logos && cd logos
```

```bash
git clone https://github.com/logos-blockchain/logos-blockchain.git
cd logos-blockchain
git fetch --tags
git checkout 0.1.2
cargo build -p logos-blockchain-node --release
./target/release/logos-blockchain-node --version
#logos-blockchain-node 0.1.2
#commit:  05f84a55 (tag 0.1.2)

mv ./target/release/logos-blockchain-node /root/logos
cd /root/logos
```

{% hint style="info" icon="sparkle" %}
For Ubuntu 24.04, you can download the binary

```bash
wget https://github.com/logos-blockchain/logos-blockchain/releases/download/0.1.2/logos-blockchain-node-linux-x86_64-0.1.2.tar.gz
tar -xzf logos-blockchain-node-linux-x86_64-0.1.2.tar.gz
rm logos-blockchain-node-linux-x86_64-0.1.2.tar.gz
chmod +x ./logos-blockchain-node
./logos-blockchain-node --version
# logos-blockchain-node 0.1.2
# commit:  05f84a5 (tag 0.1.2)
```
{% endhint %}

#### Initialize the node to create the necessary configuration files

```shell
./logos-blockchain-node init \
    -p /ip4/65.109.51.37/udp/3000/quic-v1/p2p/12D3KooWFrouXfmrR4nsLMtE7wu15DoMJ6VtoUtHinREZCvbWHar \
    -p /ip4/65.109.51.37/udp/3001/quic-v1/p2p/12D3KooWJRGau8M1rjT7R5e4YYsgdFhsMX35nRDtMwCDjxQkXAHz \
    -p /ip4/65.109.51.37/udp/3002/quic-v1/p2p/12D3KooWQXJavMDTRscjauFSgVAB1VLB6Rzpy2uY5SU9Tk7927tb \
    -p /ip4/65.109.51.37/udp/3003/quic-v1/p2p/12D3KooWSQc7CcGtvWDPF1yCbBthFnQjprfCVHmfmNDUrSmqQsU1
```



#### Create a service file

```shell
tee /etc/systemd/system/logos-node.service > /dev/null <<EOF
[Unit]
Description=Logos Blockchain Node
After=network-online.target
Wants=network-online.target

[Service]
User=$USER
WorkingDirectory=$HOME/logos
ExecStart=$HOME/logos/logos-blockchain-node /root/logos/user_config.yaml

Restart=always
RestartSec=10

LimitNOFILE=65535

Environment=RUST_BACKTRACE=1
Environment=LOG_BACKEND=stdout
#Environment=LOG_LEVEL=INFO

StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable logos-node
systemctl restart logos-node && journalctl -u logos-node -f -o cat
```

{% hint style="info" %}
Don't forget to save user\_config.yaml
{% endhint %}

Find the keys associated with your node and request tokens from the faucet for any key in known\_keys. The faucet isn't working perfectly right now—stay tuned for updates on Discord

```bash
grep -A3 known_keys $HOME/logos/user_config.yaml
```

Check your balance

```shellscript
curl -s http://localhost:8080/wallet/<your-chosen-key>/balance | jq .
```

Your tokens will be available for consensus in 3.5 hours. Ensure your node is participating by checking that the mode remains Online and the height continues to grow

Block proposals are probabilistic. Your node will not propose a block in every time slot; participation depends on the amount of stake

### Useful commands

```bash
# view key
grep -A3 known_keys $HOME/logos/user_config.yaml
# check balance
curl -w "\n" http://localhost:8080/wallet/<key>/balance
# check peers
curl -s http://127.0.0.1:8080/network/info | jq .
# check the synchronization status
curl -s http://127.0.0.1:8080/cryptarchia/info | jq .
```
