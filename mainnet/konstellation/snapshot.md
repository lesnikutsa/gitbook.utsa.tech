# ⌚ Snapshot

{% hint style="warning" %}
Be careful with snapshot during network outages. If configured incorrectly, you can get a double signature
{% endhint %}



{% hint style="success" %}
### Snapshot

time: **every 7 days** | indexer: **kv**

🌐 [**https://share106-7.utsa.tech/konstellation/**](https://share106-7.utsa.tech/konstellation/)
{% endhint %}

```shell
cd $HOME
systemctl stop knstld

cp $HOME/.knstld/data/priv_validator_state.json $HOME/.knstld/priv_validator_state.json.backup
rm -rf $HOME/.knstld/data 

curl -o - -L https://share106-7.utsa.tech/konstellation/konstellation_mainnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.knstld/

mv $HOME/.knstld/priv_validator_state.json.backup $HOME/.knstld/data/priv_validator_state.json
systemctl restart knstld && journalctl -u knstld -f -o cat
```

