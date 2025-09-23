# ⌚ Snapshot

{% hint style="success" %}
### Snapshot Archive

**Every** `24 hours`; **type:** `full node`

🌐 [https://share.utsa.tech/autonity/](https://share.utsa.tech/autonity/)
{% endhint %}

```shell
systemctl stop autonity

rm -rf $HOME/autonity-chaindata/autonity/chaindata
#rm -rf $HOME/autonity-chaindata/autonity/{chaindata,nodes,triecache,LOCK,transactions.rlp}

# скачиваем snapshot
curl -o - -L https://share.utsa.tech/autonity/autonity.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/autonity-chaindata/autonity/

systemctl restart autonity && journalctl -u autonity -f -o cat
```

