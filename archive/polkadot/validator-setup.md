# ⚙️ Validator setup

After the node has synchronized, we extract the key from our node by entering the command. If the node is not on standard ports, then we change the RPC port at the end of the command

```
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9933
```

{% hint style="info" %}
If you get a similar result, then everything is great {"jsonrpc":"2.0","result":"**0xa0very0long0hex0string**","id":1} - copy the key (highlighted in bold) we will need it soon
{% endhint %}

```bash
# check if the keys have been created
ls -a $HOME/.polkadot/chains/polkadot/keystore/
```

* Go to the [website](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fkusama-rpc.polkadot.io#/accounts) and select Network - Staking - Accounts - Validator
* Select stash accounts, stake amount and click next
* Next, we insert our key received from the validator node, select the commission percentage and sign the transaction

{% hint style="warning" %}
**Important - save the generated keys**
{% endhint %}
