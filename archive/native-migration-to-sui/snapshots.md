# ⌚ Snapshots

{% hint style="warning" %}
Be careful with snapshot during network outages. If configured incorrectly, you can get a double signature
{% endhint %}

{% hint style="success" %}
### Snapshot Pruned

time: **every 24 hours** **|** indexer: **kv |** app.toml: **goleveldb |** config.toml: **pebbledb**

🌐 [**https://share102.utsa.tech/native/**](https://share102.utsa.tech/native/)
{% endhint %}

```bash
cd $HOME
systemctl stop gonatived

cp $HOME/.gonative/data/priv_validator_state.json $HOME/.gonative/priv_validator_state.json.backup
rm -rf $HOME/.gonative/data/{application.db,evidence.db,snapshots,tx_index.db,blockstore.db,state.db,cs.wal}

curl -o - -L https://share102.utsa.tech/native/native_testnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.gonative/
mv $HOME/.gonative/priv_validator_state.json.backup $HOME/.gonative/data/priv_validator_state.json

systemctl restart gonatived && journalctl -u gonatived -f -o cat
```
