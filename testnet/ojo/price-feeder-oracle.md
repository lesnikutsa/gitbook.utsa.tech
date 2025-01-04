# 📬 Price feeder (✔️Oracle)

Docs - [https://github.com/ojo-network/price-feeder](https://github.com/ojo-network/price-feeder)

{% hint style="success" %}
In this example:&#x20;

* a separate wallet is used (not the validator's wallet)
* keyring-backend set os
* a separate wallet is attached to the validator and Environment="PRICE\_FEEDER\_PASS=$PASS" is added to the service file
* a separate wallet should have coins for commissions
* the validator must be in the active set
{% endhint %}

```shell
# install binary
cd $HOME
git clone https://github.com/ojo-network/price-feeder && cd price-feeder
git checkout v0.1.1
make install

price-feeder version
# version: HEAD-5d46ed438d33d7904c0d947ebc6a3dd48ce0de59
# commit: 5d46ed438d33d7904c0d947ebc6a3dd48ce0de59
# sdk: v0.46.7
# go: go1.19.4 linux/amd64
```

**Create a directory and download the default config**

```sh
mkdir -p $HOME/price-feeder_config
wget -O $HOME/price-feeder_config/price-feeder.toml "https://raw.githubusercontent.com/ojo-network/price-feeder/main/price-feeder.example.toml"
```

**Create a separate wallet for Feeder and replenish its balance**

```sh
ojod keys add OJO_FEEDER_ADDR --keyring-backend os
ojod tx bank send <name_wallet> <addr_wallet> 100000000uojo --fees 20000uojo
```

**Set variables**

```shell
PASS=<your_password>
OJO_ADDR=<ojo13y...>
OJO_FEEDER_ADDR=<ojo1jkg...>
OJO_VALOPER=<ojovaloper13y7...>
OJO_CHAIN=ojo-devnet
```

**Set up price-feeder.toml (if necessary, change RPC and gRPC ports)**

```sh
sed -i '/^dir *=.*/a pass = ""' $HOME/price-feeder_config/price-feeder.toml

sed -i "s/^address *=.*/address= \"$OJO_ADDR\"/;\
s/^chain_id *=.*/chain_id= \"$OJO_CHAIN\"/;\
s/^validator *=.*/validator = \"$OJO_VALOPER\"/;\
s/^backend *=.*/backend = \"os\"/;\
s|^dir *=.*|dir = \"$HOME/.ojo\"|;\
s|^pass *=.*|pass = \"$PASS\"|;\
s|^grpc_endpoint *=.*|grpc_endpoint = \"localhost:9090\"|;\
s|^tmrpc_endpoint *=.*|tmrpc_endpoint = \"http://localhost:26657\"|;" $HOME/price-feeder_config/price-feeder.toml
```

**Create a service file**

```sh
tee /etc/systemd/system/price-feeder.service > /dev/null <<EOF
[Unit]
Description=OJO PFD
After=network.target
[Service]
User=$USER
Environment="PRICE_FEEDER_PASS=$PASS"
Type=simple
ExecStart=$(which price-feeder) $HOME/price-feeder_config/price-feeder.toml --log-level debug
RestartSec=10
Restart=on-failure
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

```sh
systemctl daemon-reload
systemctl enable price-feeder
systemctl restart price-feeder && journalctl -u price-feeder -f -o cat
```

**If your validator is not in the active set, you will see the following logs:**

<figure><img src="https://img4.teletype.in/files/72/dc/72dc0260-4bea-4ab9-854e-59fe31304207.png" alt=""><figcaption></figcaption></figure>

**Once the validator is in the active set, the logs will be as follows:**

<figure><img src="https://img1.teletype.in/files/09/40/0940830a-a4c2-4113-a189-694f9e04b971.png" alt=""><figcaption></figcaption></figure>

**Delegate authority to a separately created wallet OJO\_FEEDER\_ADDR**

```sh
ojod tx oracle delegate-feed-consent $OJO_ADDR $OJO_FEEDER_ADDR --fees 40000uojo
```

**Making changes to price-feeder.toml**

```sh
sed -i "s/^address *=.*/address= \"$OJO_FEEDER_ADDR\"/" $HOME/price-feeder_config/price-feeder.toml

systemctl restart price-feeder && journalctl -u price-feeder -f -o cat
```
