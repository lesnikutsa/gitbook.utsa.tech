# 🔧 Creating / Editing a Validator

## General information

For all blockchains in the Cosmos ecosystem, almost the same commands work - therefore, the examples below can be used for different blockchains - only changing identifiers



#### **First, let's understand the main keys**&#x20;

* **--from** the name of the local key (wallet) that belongs to the validator and has a certain number of coins in the account
* -**-amount** amount of coins to be placed in own validator (own stake)
* **--pubkey** public key of the validator
* **--moniker** unique validator name under which you will be seen in the list of validators
* **--security-contact** email or other validator ID that can be used to contact the validator
* **--details** short description of the validator (any arbitrary text)
* **--website** link to any resource available to the validator
* **--identity** specifying a 16-digit Keybase identifier will allow using the Keybase API to bind an avatar from Keybase to the validator
* **--min-self-delegation** The minimum amount of the validator's own stake that must remain in the account. If the number of self-delegated coins falls below this amount, the validator becomes inactive. A large amount can mean serious ambitions of the validator and his increased responsibility to the delegators, which will be a plus when choosing this validator. The sum of 1000000 will be equal to 1 coi
* &#x20;**--commission-rate** percentage of profit a validator receives from the sum of its delegates' rewards (commission charged). The number 1 will be equal to 100% of the validator's commission, while delegators will not receive anything at all by delegating to this validator. And for example, 0.1 will equal 10% of the validator's commission, which means that before returning the profit to the delegates, the validator takes 10% of it in his favor
* **--commission-max-rate** is the maximum possible validator commission. This parameter remains unchanged and is set only when the validator is created. The number 1 will be equal to 100% of the validator's commission, and for example 0.1 will be equal to 10% of the validator's commission
* -**-commission-max-change-rate** percentage by which a validator can change its commission within 1 day. You can both reduce the commission and increase it, but until the
* **--commission-max-rate**  For example 0.01 will mean that the validator will be able to change the fee by 1 percent per day
* **--chain-id** network identifier. Can be both testnet and mainnet
* **--gas** gas limit for each transaction. "auto" to automatically calculate enough gas
* **--fees** fees payable with the transaction, e.g. 5denom

{% hint style="warning" %}
To create a validator you need:

* fully synchronized node
* wallet with some coins
{% endhint %}

## Create a validator

{% hint style="warning" %}
Please enter your data into the variables. If necessary, not all keys can be used

The data below is an example!
{% endhint %}

```shell
BINARY="canined"
CHAIN="jackal-1"
DENOM="ujkl"
MONIKER="Cosmos master"
WALLET="test"
IDENTITY=""
DETAILS=""
CONTACT=""
WEB=""
```

```shell
$BINARY tx staking create-validator \
--chain-id="$CHAIN" \
--commission-rate=0.1 \
--commission-max-rate=0.1 \
--commission-max-change-rate=0.01 \
--amount=1000000$DENOM \
--pubkey $($BINARY tendermint show-validator) \
--moniker="$MONIKER" \
--details="$DETAILS" \
--security-contact="$CONTACT" \
--website="$WEB" \
--identity="$IDENTITY" \
--min-self-delegation="1000000" \
--from="$WALLET" \
--fees 5000$DENOM
```

## Editing a validator

```shell
$BINARY tx staking edit-validator \
--moniker="$MONIKER" \
--identity="$IDENTITY" \
--details="$DETAILS" \
--chain-id="$CHAIN" \
--from="$WALLET" \
--commission-rate="0.09" \
--fees 5000$DENOM
```
