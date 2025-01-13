# ⌚ Snapshots

{% hint style="warning" %}
Be careful with snapshot during network outages. If configured incorrectly, you can get a double signature
{% endhint %}

{% hint style="success" %}
### Snapshot Pruned

time: **every 24 hours** **|** indexer: **kv |** pruning: **1000/100**

🌐 [https://share102.utsa.tech/atomone/](https://share102.utsa.tech/atomone/)
{% endhint %}

```bash
cd $HOME
systemctl stop atomoned

cp $HOME/.atomone/data/priv_validator_state.json $HOME/.atomone/priv_validator_state.json.backup
rm -rf $HOME/.atomone/data/{application.db,evidence.db,snapshots,tx_index.db,blockstore.db,state.db,cs.wal}


curl -o - -L https://share102.utsa.tech/atomone/atomone.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.atomone/
mv $HOME/.atomone/priv_validator_state.json.backup $HOME/.atomone/data/priv_validator_state.json

systemctl restart atomoned && journalctl -u atomoned -f -o cat
```
