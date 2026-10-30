# 📡 Creating a validator

{% hint style="info" %}
**Important:** please create a wallet after the node is fully synchronized and top it up using a method convenient for you
{% endhint %}

#### Create a wallet

```shell
# create wallet
luggomd keys add <name_wallet>

# restore wallet (insert seed after command)
luggomd keys add <wallet_name> --recover

# check balance
luggomd q bank balances <address>
```

{% hint style="danger" %}
**Important:** please save your seed phrase in a safe place
{% endhint %}

#### Creating a validator

```shell
# get the pubkey
luggomd comet show-validator
#

# create validator.json
nano $HOME/.luggom/validator.json

{
  "pubkey": {"@type":"/cosmos.crypto.ed25519.PubKey","key":"<from: luggomd comet show-validator>"},
  "amount": "200000000000000000000uluggom",
  "moniker": "UTSA-guide",
  "identity": "",
  "website": "",
  "security": "",
  "details": "",
  "commission-rate": "0.05",
  "commission-max-rate": "0.20",
  "commission-max-change-rate": "0.01",
  "min-self-delegation": "1"
}
  
  # send transaction
luggomd tx staking create-validator $HOME/.luggom/validator.json --from <wallet_name> --chain-id luggom-test --gas auto --gas-adjustment 1.3 --gas-prices 1uluggom -y
```

{% hint style="danger" %}
**Important:** please save your priv\_validator\_key
{% endhint %}



