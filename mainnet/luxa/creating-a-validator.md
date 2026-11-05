# 📡 Creating a validator

{% hint style="info" %}
**Important:** please create a wallet after the node is fully synchronized and top it up using a method convenient for you
{% endhint %}

#### Create a wallet

```shell
# create wallet
luxad keys add <name_wallet>

# restore wallet (insert seed after command)
luxad keys add <wallet_name> --recover

# check balance
luxad q bank balances <address>
```

{% hint style="danger" %}
**Important:** please save your seed phrase in a safe place
{% endhint %}

{% hint style="danger" %}
**Once you have created your wallet and your node has synchronized, visit the project website to review the next steps for receiving tokens: https://luxaecosystem.xyz/validator-guide.**\
**Please read the instructions carefully—you will definitely need a LinkedIn account**
{% endhint %}

#### Creating a validator

```shell
# get the pubkey
luxad comet show-validator
#

# create validator.json
nano $HOME/.luxa/validator.json

{
  "pubkey": {"@type":"/cosmos.crypto.ed25519.PubKey","key":"<from: luxad comet show-validator>"},
  "amount": "5000000uluxa",
  "moniker": "UTSA-guide",
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
luxad tx staking create-validator $HOME/.luxa/validator.json --from <wallet_name> --chain-id luxa-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.0025uluxa -y
```

{% hint style="danger" %}
**Important:** please save your priv\_validator\_key
{% endhint %}



