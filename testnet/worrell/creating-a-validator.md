# 📡 Creating a validator

{% hint style="info" %}
**Important:** please create a wallet after the node is fully synchronized and top it up using a method convenient for you
{% endhint %}

#### Create a wallet

```shell
# create wallet
worrelld keys add <name_wallet>

# restore wallet (insert seed after command)
worrelld keys add <wallet_name> --recover

# check balance
worrelld q bank balances <address>
```

{% hint style="danger" %}
**Important:** please save your seed phrase in a safe place
{% endhint %}

#### Creating a validator

```shell
# get the pubkey
worrelld comet show-validator
#

# create validator.json
nano $HOME/.worrell/validator.json

{
  "pubkey": {"@type":"/cosmos.crypto.ed25519.PubKey","key":"<from: worrelld comet show-validator>"},
  "amount": "1000000uworrell",
  "moniker": "community-1",
  "identity": "",
  "website": "",
  "security": "",
  "details": "",
  "commission-rate": "0.10",
  "commission-max-rate": "0.20",
  "commission-max-change-rate": "0.01",
  "min-self-delegation": "1"
}
  
  # send transaction
worrelld tx staking create-validator $HOME/.worrell/validator.json \
  --from <wallet_name> --chain-id worrell-testnet-1 --gas auto --gas-adjustment 1.3 --fees 20000uworrell -y
```

{% hint style="danger" %}
**Important:** please save your priv\_validator\_key
{% endhint %}



