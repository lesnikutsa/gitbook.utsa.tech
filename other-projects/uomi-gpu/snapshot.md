# ⌚ Snapshot

{% hint style="success" %}
### Snapshot

**Every** `24 hours`; **pruning:** `1000`

🌐 soon
{% endhint %}

```shell
systemctl stop uomi

rm -r $HOME/uomi/chains/uomi/db/

#curl -o - -L https://share.utsa.tech/uomi/uomi-testnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/uomi/chains/uomi/

systemctl restart uomi && journalctl -u uomi -f -o cat
```

