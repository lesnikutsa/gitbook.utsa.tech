# ⌚ Snapshot

{% hint style="success" %}
### Snapshot Archive

**Every** `24 hours`; **pruning:** `archive`

🌐 [**https://share.utsa.tech/avail-testnet/**](https://share.utsa.tech/avail-testnet/)
{% endhint %}

```shell
systemctl stop avail

# delete the database. If necessary, change the path to your own
rm -r $HOME/.avail/data/chains/avail_turing_network/paritydb/

# download snapshot. If necessary, change the path to your own
curl -o - -L https://share.utsa.tech/avail-testnet/avail-archive.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.avail/data/chains/avail_turing_network/

systemctl restart avail && journalctl -u avail -f -o cat
```

