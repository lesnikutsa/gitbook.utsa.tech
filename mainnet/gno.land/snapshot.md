# ⌚ Snapshot

{% hint style="success" %}
### Snapshot

**Every** `6 hours`; **db\_backend:** pebbledb

🌐 [**https://share.utsa.tech/gno\_main/**](https://share.utsa.tech/gno_main/)
{% endhint %}

```shell
# stop the service
systemctl stop gnoland 2>/dev/null || pkill -f "gnoland start" 2>/dev/null || true

# clearing the old database
rm -rf ~/gno/gnoland-data/db ~/gno/gnoland-data/wal

# download snapshot
curl -o - -L https://share.utsa.tech/gno_main/gno.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/gno/gnoland-data/
```

```shell
systemctl restart gnoland && journalctl -u gnoland -f -o cat
```





