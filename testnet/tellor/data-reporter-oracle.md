# 📬 Data Reporter (✔️Oracle)

Docs - [https://docs.tellor.io/layer-docs/public-testnet/run-the-data-reporter](https://docs.tellor.io/layer-docs/public-testnet/run-the-data-reporter)

{% hint style="success" %}
It is assumed that you already have a node and validator running



* A `commision-rate` of `0.02` means that you get 2% of rewards from your selectors.
* A `min-tokens-required` value of `1000000` prevents spam by requiring that selectors have at least 1 TRB in their wallet.
{% endhint %}

1. **Run the create-reporter command**

```shell
# layerd tx reporter create-reporter [commission-rate] [min-tokens-required] [flags]
layerd tx reporter create-reporter 0.25 1000000 --from <name_wallet> --chain-id layertest-3 --fees 5loya --yes
```

2. **Check if your reporter has been created successfully. If you see your address in the list, then your reporter has been created successfully**

```sh
layerd query reporter reporters | grep <tellor1k8v5l...>
```

3. **In the service file, change the startup line and add Environments. Replace everything in <> with your own values**

```sh
nano /etc/systemd/system/layerd.service
```

```bash
[Unit]
Description=layerd
After=network-online.target

[Service]
User=root
ExecStart=/root/go/bin/layerd start --api.enable --api.swagger --price-daemon-enabled=true --panic-on-daemon-failure-enabled=false --home /root/.layer --key-name <name_wallet> --keyring-backend test
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
Environment="WITHDRAW_FREQUENCY=21600"
Environment="REPORTERS_VALIDATOR_ADDRESS=<tellorvaloper1k8v...>"
Environment="ETH_RPC_URL=<wss://a.good.sepolia.rpc.url>"
Environment="TOKEN_BRIDGE_CONTRACT=0x6ac02f3887b358591b8b2d22cfb1f36fa5843867"

[Install]
WantedBy=multi-user.target
```

```bash
systemctl daemon-reload
systemctl restart layerd && journalctl -u layerd -f -o cat
```

4. **Check successful oracle transactions**

```bash
layerd query oracle get-reportsby-reporter <tellor1k8v5l...>
```

