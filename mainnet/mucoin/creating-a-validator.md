# 📡 Creating a validator

{% hint style="info" %}
**Important:** please create a wallet after the node is fully synchronized and top it up using a method convenient for you
{% endhint %}

#### Create a wallet

```shell
# create wallet
mucoind keys add <wallet_name>

# restore wallet (insert seed after command)
mucoind keys add <wallet_name> --recover

# check balance
mucoind q bank balances <address>
```

{% hint style="danger" %}
**Important:** please save your seed phrase in a safe place
{% endhint %}

#### Creating a validator

```shell
# get the pubkey
PUBKEY_JSON="$(mucoind comet show-validator)"
echo $PUBKEY_JSON

# create validator.json
jq -n \
  --argjson pubkey "$PUBKEY_JSON" \
  '{
    "pubkey": $pubkey,
    "amount": "1000000umuc",
    "moniker": "<moniker>",
    "identity": "",
    "website": "",
    "security": "",
    "details": "",
    "commission-rate": "0.05",
    "commission-max-rate": "0.777",
    "commission-max-change-rate": "0.1",
    "min-self-delegation": "1"
  }' > "$HOME/.mucoin/validator.json"
  
  # send transaction
  mucoind tx staking create-validator "$HOME/.mucoin/validator.json" \
  --chain-id mucoin-1 \
  --from "wallet" \
  --gas auto \
  --gas-adjustment 1.5 \
  --gas-prices 0.005umuc \
  -y
```

{% hint style="danger" %}
**Important:** please save your priv\_validator\_key
{% endhint %}



