# 💻 Create a validator

**Create or restore a wallet**

```bash
# create a wallet
zenrockd keys add <name_wallet> --keyring-backend os

# restore wallet (after the command insert seed)
zenrockd keys add <name_wallet> --recover --keyring-backend os
```

#### First, let's look at the public validator keys

```bash
zenrockd comet show-validator
# {"@type":"/cosmos.crypto.ed25519.PubKey","key":"ZXONS7NNjLWH4HePBOoHKDAYeLXQO5iUwpCRQSi1poI="}
```

**Create validator.json**

```bash
nano $HOME/.zrchain/validator.json
```

```bash
{
  "pubkey": {#pubkey},
  "amount": "1000000urock",
  "moniker": "moniker",
  "identity": "",
  "website": "",
  "security": "",
  "details": "",
  "commission-rate": "0.05",
  "commission-max-rate": "0.25",
  "commission-max-change-rate": "0.05",
  "min-self-delegation": "1"
}
```

**Sending a transaction**

```bash
zenrockd tx validation create-validator $HOME/.zrchain/validator.json \
    --from=<key-name> \
    --chain-id=gardia-2 \
    --gas auto --gas-adjustment 1.5 --gas-prices 27urock
```

{% hint style="warning" %}
**Don't forget to save priv\_validator\_key.json**&#x20;
{% endhint %}

