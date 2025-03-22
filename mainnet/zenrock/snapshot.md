# ⌚ Snapshot

{% hint style="warning" %}
Be careful with snapshot during network outages. If configured incorrectly, you can get a double signature
{% endhint %}



{% hint style="success" %}
type: **pruned |** time: **every 1 days |** pruning: **1000/10 |** indexer: **kv**

🌐 [**https://share.utsa.tech/zenrock**](https://share.utsa.tech/zenrock)
{% endhint %}

```shell
systemctl stop zenrockd

cp $HOME/.zrchain/data/priv_validator_state.json $HOME/.zrchain/priv_validator_state.json.backup

rm -rf $HOME/.zrchain/data
rm -rf $HOME/.zrchain/wasm

# скачиваем
curl -o - -L https://share.utsa.tech/zenrock/zenrock.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.zrchain/

mv $HOME/.zrchain/priv_validator_state.json.backup $HOME/.zrchain/data/priv_validator_state.json

systemctl restart zenrockd
```

