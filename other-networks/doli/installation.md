# 💻 Installation

## Server preparation

```shell
apt update && sudo apt upgrade -y
```

```shell
apt install -y git curl wget jq build-essential pkg-config libssl-dev libgmp-dev librocksdb-dev protobuf-compiler
```

**Rust**

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env

rustc --version
cargo --version
```

**UFW**

```bash
ufw allow 30300/tcp
ufw allow 30301/udp
```

## Node installation

```shell
cd 
git clone https://github.com/doli-network/doli && cd $HOME/doli
git checkout main
git pull
grep '^version' Cargo.toml | head -1
# version = "6.43.0"

cargo build --release -p doli-node -p doli-cli

cp $HOME/doli/target/release/doli-node /usr/local/bin/doli-node
cp $HOME/doli/target/release/doli /usr/local/bin/doli
chmod +x /usr/local/bin/doli-node /usr/local/bin/doli

doli-node --version
doli --version
# doli-node 6.43.0 (e5a513f7)
# doli 6.43.0 (e5a513f7)
```

#### Initialize the data directory

```shell
mkdir -p $HOME/.doli/mainnet
doli-node --network mainnet --data-dir $HOME/.doli/mainnet init
```

#### Creating a wallet

```shell
doli -w $HOME/.doli/mainnet/wallet.json init

# restore wallet
doli -w $HOME/.doli/mainnet/wallet.json --network mainnet restore
```

{% hint style="warning" %}
**Don't forget to save your seed and wallet files!!!**
{% endhint %}

#### Creating a service file

```shell
SERVER_IP=$(curl -4 -s ifconfig.me); USER_NAME=$(id -un); HOME_DIR=$HOME; sudo tee /etc/systemd/system/doli.service > /dev/null <<EOF
[Unit]
Description=DOLI Mainnet Producer
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=${USER_NAME}
ExecStart=/usr/local/bin/doli-node \
  --network mainnet \
  --data-dir ${HOME_DIR}/.doli/mainnet \
  --log-level info \
  run \
  --producer \
  --producer-key ${HOME_DIR}/.doli/mainnet/wallet.json \
  --p2p-port 30300 \
  --discv5-port 30301 \
  --external-address /ip4/${SERVER_IP}/tcp/30300 \
  --rpc-port 8500 \
  --metrics-port 9000 \
  --bootstrap /dns4/seed1.doli.network/tcp/30300 \
  --bootstrap /dns4/seed2.doli.network/tcp/30300 \
  --bootstrap /dns4/seed3.doli.network/tcp/30300

Restart=always
RestartSec=10
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable doli
systemctl start doli && journalctl -u doli -f -o cat
```



## Registration producer

To register a producer, a minimum of 1 bond (10 DOLI) is required.

You can obtain 10.000001 DOLI via the faucet. This covers the cost of one bond and the registration fee. The faucet can be used once per person and is intended for a new, empty address.

Go to Discord and do the following:

```shell
/faucet
```

The bot will provide a private link; follow the instructions after clicking on it

**Register as a Producer**

```shell
doli -w $HOME/.doli/mainnet/wallet.json \
  --rpc http://127.0.0.1:8500 \
  --network mainnet \
  producer register --bonds 1
```

Registration does not take effect immediately; the change to the producer set occurs at the boundary of the next epoch

```shell
# registration check
doli -w $HOME/.doli/mainnet/wallet.json \
  --rpc http://127.0.0.1:8500 \
  --network mainnet \
  producer status
```

#### Registering the name Producer

After registering the producer, you can associate a readable name with it

[https://doli.network/register.html](https://doli.network/register.html)

The page will generate a challenge that must be signed with your producer wallet

