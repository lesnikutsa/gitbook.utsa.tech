# ⌚ Snapshot



{% hint style="success" %}
### Snapshot Pruned

**every 24 hours** **| pruned | RocksDB**

🌐 [**https://share.utsa.tech/polkadot/**](https://share.utsa.tech/polkadot/)
{% endhint %}

```shell
docker stop polkadot_node
docker rm polkadot_node

rm -r $HOME/.polkadot/chains/polkadot/db/

curl -o - -L https://share.utsa.tech/polkadot/polkadot_pruned.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.polkadot/chains/polkadot/

# we launch as usual
```

{% hint style="success" %}
### Snapshot KAGOM

**every 7 days** **| pruned | KAGOM**

🌐 **http://205.209.107.2:8000**
{% endhint %}

```shell
curl -o - -L http://205.209.107.2:8000/polkadot/polkadot_kagom_pruned.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.polkadot/chains/polkadot/
```
