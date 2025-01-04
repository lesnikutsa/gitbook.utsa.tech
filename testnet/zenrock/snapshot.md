# ⌚ Snapshot

{% hint style="warning" %}
Be careful with snapshot during network outages. If configured incorrectly, you can get a double signature
{% endhint %}



{% hint style="success" %}
type: **archive |** time: **every 1 days |** pruning: **nothing |** indexer: **kv**

🌐 [**https://share106-7.utsa.tech/zenrock/**](https://share106-7.utsa.tech/zenrock/)
{% endhint %}

```shell
systemctl stop zenrockd

cp $HOME/.zrchain/data/priv_validator_state.json $HOME/.zrchain/priv_validator_state.json.backup

rm -rf $HOME/.zrchain/data
rm -rf $HOME/.zrchain/wasm

curl -o - -L https://share106-7.utsa.tech/zenrock/zenrock_archive_testnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.zrchain/

mv $HOME/.zrchain/priv_validator_state.json.backup $HOME/.zrchain/data/priv_validator_state.json

systemctl restart zenrockd
```

