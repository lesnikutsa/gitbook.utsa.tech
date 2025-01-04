# ⌚ Snapshot

{% hint style="warning" %}
Be careful with snapshot during network outages. If configured incorrectly, you can get a double signature
{% endhint %}



**Every 24 hours**; pruning: **custom/1000/0/10**; indexer: kv

```shell
# install lz4
apt update
apt install snapd -y
snap install lz4
```

```shell
systemctl stop lavad

cp $HOME/.lava/data/priv_validator_state.json $HOME/.lava/priv_validator_state.json.backup
lavad tendermint unsafe-reset-all --home $HOME/.lava --keep-addr-book

curl -L https://share101.utsa.tech/lava/snap-lava.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.lava --strip-components 2

mv $HOME/.lava/priv_validator_state.json.backup $HOME/.lava/data/priv_validator_state.json

systemctl restart lavad && journalctl -u lavad -f -o cat
```

