# ⌚ Snapshot



{% hint style="success" %}
### Snapshot Pruned

**every 24 hours** **| pruned | RocksDB**

🌐 [**https://share.utsa.tech/kusama/**](https://share106-3.utsa.tech/kusama/)
{% endhint %}

```shell
docker stop kusama_node
docker rm kusama_node

rm -r /root/.kusama/chains/ksmcc3/db/

curl -o - -L https://share.utsa.tech/kusama/kusama_pruned.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.kusama/chains/ksmcc3/

# we launch as usual
```

{% hint style="success" %}
### Snapshot KAGOM

**every 7 days** **| pruned | KAGOM**

🌐 **http://205.209.107.2:8000/kusama**
{% endhint %}

```shell
curl -o - -L http://205.209.107.2:8000/kusama/kusama_kagom_pruned.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.kusama_kagome/chains/ksmcc3/
```
