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
# install binary
mkdir -p $HOME/.nibid/pricefeeder && cd $HOME/.nibid/pricefeeder
wget https://github.com/NibiruChain/pricefeeder/releases/download/v0.21.6/pricefeeder_0.21.6_linux_amd64.tar.gz
tar -xvf pricefeeder_0.21.6_linux_amd64.tar.gz
mv pricefeeder /usr/local/bin/

sha256sum /usr/local/bin/pricefeeder
# 08a84a475a035efe1a737c499964a7206a2caa1b367fd1d25ea985a6d8dd30ad
```

```shell
# set variables
export CHAIN_ID="nibiru-itn-3"
export GRPC_ENDPOINT="localhost:9090"
export WEBSOCKET_ENDPOINT="ws://localhost:26657/websocket"
export EXCHANGE_SYMBOLS_MAP='{"bitfinex":{"ubtc:unusd":"tBTCUSD","ubtc:uusd":"tBTCUSD","ueth:unusd":"tETHUSD","ueth:uusd":"tETHUSD","uusdc:uusd":"tUDCUSD","uusdc:unusd":"tUDCUSD"},"coingecko":{"ubtc:uusd":"bitcoin","ubtc:unusd":"bitcoin","ueth:uusd":"ethereum","ueth:unusd":"ethereum","uusdt:uusd":"tether","uusdt:unusd":"tether","uusdc:uusd":"usd-coin","uusdc:unusd":"usd-coin","uatom:uusd":"cosmos","uatom:unusd":"cosmos","ubnb:uusd":"binancecoin","ubnb:unusd":"binancecoin","uavax:uusd":"avalanche-2","uavax:unusd":"avalanche-2","usol:uusd":"solana","usol:unusd":"solana","uada:uusd":"cardano","uada:unusd":"cardano"}}'
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
