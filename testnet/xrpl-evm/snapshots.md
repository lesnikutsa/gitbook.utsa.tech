# ⌚ Snapshots

{% hint style="warning" %}
Be careful with snapshot during network outages. If configured incorrectly, you can get a double signature
{% endhint %}

{% hint style="success" %}
### Snapshot Pruned

time: **every 24 hours** **|** indexer: **kv |** pruning: **1000/100**

🌐 [**https://share102.utsa.tech/xrpl/**](https://share102.utsa.tech/xrpl/)
{% endhint %}

```bash
cd $HOME
systemctl stop exrpd

cp $HOME/.exrpd/data/priv_validator_state.json $HOME/.exrpd/priv_validator_state.json.backup

rm -rf $HOME/.exrpd/data/{application.db,evidence.db,snapshots,tx_index.db,blockstore.db,state.db,cs.wal}

curl -o - -L https://share102.utsa.tech/xrpl/xrpl.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.exrpd/

mv $HOME/.exrpd/priv_validator_state.json.backup $HOME/.exrpd/data/priv_validator_state.json

systemctl restart exrpd && journalctl -u exrpd -f -o cat
```
