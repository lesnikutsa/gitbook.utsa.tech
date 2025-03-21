# ⌚ Snapshot



{% hint style="success" %}
### Snapshot Pruned

**every 24 hours** **| pruned | RocksDB**

🌐 [**https://share106-3.utsa.tech/polkadot/**](https://share106-3.utsa.tech/polkadot/)
{% endhint %}

```shell
docker stop polkadot_node
docker rm polkadot_node

rm -r $HOME/.polkadot/chains/polkadot/db/

curl -o - -L https://share.utsa.tech/polkadot/polkadot_pruned.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.polkadot/chains/polkadot/

# we launch as usual
```

{% hint style="success" %}
### Snapshot Archive

**every 7 days** **| archive | RocksDB**

🌐 [**https://share106-7.utsa.tech/polkadot/**](https://share106-7.utsa.tech/polkadot/)
{% endhint %}

```shell
docker stop polkadot_node
docker rm polkadot_node

rm -r $HOME/.polkadot/chains/polkadot/db/

curl -o - -L https://share106-7.utsa.tech/polkadot/polkadot_archive.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.polkadot/chains/polkadot/

# we launch as usual
```
