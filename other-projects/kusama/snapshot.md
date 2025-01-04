# ⌚ Snapshot



{% hint style="success" %}
### Snapshot Pruned

**every 24 hours** **| pruned | RocksDB**

🌐 [**https://share106-3.utsa.tech/kusama/**](https://share106-3.utsa.tech/kusama/)
{% endhint %}

```shell
docker stop kusama_node
docker rm kusama_node

rm -r /root/.kusama/chains/ksmcc3/db/

curl -o - -L https://share106-3.utsa.tech/kusama/kusama_pruned.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.kusama/chains/ksmcc3/

# we launch as usual
```

{% hint style="success" %}
### Snapshot Archive

**every 7 days** **| archive | RocksDB**

🌐 [**https://share106-7.utsa.tech/kusama/**](https://share106-7.utsa.tech/kusama/)
{% endhint %}

```shell
docker stop kusama_node
docker rm kusama_node

rm -r /root/.kusama/chains/ksmcc3/db/

curl -o - -L https://share106-7.utsa.tech/kusama/kusama_archive.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.kusama/chains/ksmcc3/

# we launch as usual
```
