# 📬 Price feeder (✔️Oracle)

Docs - [https://nibiru.fi/docs/run-nodes/validators/pricefeeder.html](https://nibiru.fi/docs/run-nodes/validators/pricefeeder.html)

{% hint style="success" %}
In this example:&#x20;

* a separate wallet is used (not the validator's wallet)
* a separate wallet is attached to the validator and `Environment=VALIDATOR_ADDRESS='<nibivaloper1....>'` is added to the service file&#x20;
* a separate wallet should have coins for commissions&#x20;
* the validator must be in the active set
{% endhint %}

```shell

mkdir -p $HOME/.nibid/pricefeeder && cd $HOME/.nibid/pricefeeder
wget https://github.com/NibiruChain/pricefeeder/releases/download/v1.1.3/pricefeeder_1.1.3_linux_amd64.tar.gz
tar -xvf pricefeeder_1.1.3_linux_amd64.tar.gz
mv pricefeeder /usr/local/bin/

sha256sum /usr/local/bin/pricefeeder
#1ff1bb53037ea88c5ac6390b657bc85fd188546f0396dd4d4f3fc1267231943d
```

```shell
# set variables
export CHAIN_ID="cataclysm-1"
export GRPC_ENDPOINT="localhost:9090"
export WEBSOCKET_ENDPOINT="ws://localhost:26657/websocket"
export EXCHANGE_SYMBOLS_MAP='{"bitfinex":{"ubtc:unusd":"tBTCUSD","ubtc:uusd":"tBTCUSD","ueth:unusd":"tETHUSD","ueth:uusd":"tETHUSD","uusdc:uusd":"tUDCUSD","uusdc:unusd":"tUDCUSD"}}'
export FEEDER_MNEMONIC="<your mnemonic here>"
export VALIDATOR_ADDRESS="nibi1valoper..."
```

```sh
tee /etc/systemd/system/pricefeeder.service<<EOF
[Unit]
Description=Nibiru Pricefeeder
Requires=network-online.target
After=network-online.target

[Service]
Type=exec
User=$USER
ExecStart=/usr/local/bin/pricefeeder
Restart=on-failure
ExecReload=/bin/kill -HUP $MAINPID
KillSignal=SIGTERM
PermissionsStartOnly=true
LimitNOFILE=65535
Environment=CHAIN_ID='$CHAIN_ID'
Environment=GRPC_ENDPOINT='$GRPC_ENDPOINT'
Environment=WEBSOCKET_ENDPOINT='$WEBSOCKET_ENDPOINT'
Environment=EXCHANGE_SYMBOLS_MAP='$EXCHANGE_SYMBOLS_MAP'
Environment=FEEDER_MNEMONIC='$FEEDER_MNEMONIC'
Environment=VALIDATOR_ADDRESS='$VALIDATOR_ADDRESS'

[Install]
WantedBy=multi-user.target
EOF
```

```sh
systemctl daemon-reload
systemctl enable pricefeeder
systemctl restart pricefeeder && journalctl -u pricefeeder -f -o cat
```

**Bind a separate wallet with the following command**

```sh
nibid tx oracle set-feeder <nibi_address> --from <name_vallet> --fees 5000unibi -y
```

![](<../../.gitbook/assets/image (32).png>)
