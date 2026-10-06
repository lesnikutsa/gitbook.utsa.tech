# ⌚ Snapshot

###

{% hint style="success" %}
### Snapshot Archive

**Every** `24 hours`; **pruning:** `archive`

🌐 [https://share.utsa.tech/avail\_mainnet](https://share.utsa.tech/avail_mainnet)
{% endhint %}

```shell
systemctl stop avail-mainnet

# delete the database. If necessary, change the path to your own
rm -r $HOME/.avail_mainnet/data/chains/avail_da_mainnet/paritydb/

# download snapshot. If necessary, change the path to your own
curl -o - -L https://share.utsa.tech/avail_mainnet/avail-archive.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.avail_mainnet/data/chains/avail_da_mainnet/

systemctl restart avail-mainnet && journalctl -u avail-mainnet -f -o cat
```

