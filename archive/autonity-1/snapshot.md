# ⌚ Snapshot

{% hint style="success" %}
### Snapshot Archive

**Every** `24 hours`; **type:** `full node`

🌐&#x20;
{% endhint %}

```shell
systemctl stop autonity

rm -rf $HOME/autonity-chaindata/autonity/chaindata
#rm -rf $HOME/autonity-chaindata/autonity/{chaindata,nodes,triecache,LOCK,transactions.rlp}

# скачиваем snapshot
curl -o - -L https://share102.utsa.tech/autonity/autonity_testnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/autonity-chaindata/autonity/

systemctl restart autonity && journalctl -u autonity -f -o cat
```

