# ⌚ Snapshots

{% hint style="warning" %}
Be careful with snapshot during network outages. If configured incorrectly, you can get a double signature
{% endhint %}

## Consensus Node

{% hint style="success" %}
### Snapshot Pruned

time: **every 6 hours** **|** pruning: **custom: 100/10 |** indexer: **null**

🌐 [**https://share106-3.utsa.tech/celestia/**](https://share106-3.utsa.tech/celestia/)
{% endhint %}

```shell
cd $HOME
systemctl stop celestia-appd

cp $HOME/.celestia-app-main/data/priv_validator_state.json $HOME/.celestia-app-main/priv_validator_state.json.backup

rm -rf $HOME/.celestia-app-main/data

curl -o - -L https://share106-3.utsa.tech/celestia/celestia_pruned_mainnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.celestia-app-main/

mv $HOME/.celestia-app-main/priv_validator_state.json.backup $HOME/.celestia-app-main/data/priv_validator_state.json

systemctl restart celestia-appd && journalctl -u celestia-appd -f -o cat
```

{% hint style="success" %}
### Snapshot Archive

time: **every 3 days** **|** pruning: **nothing |** indexer: **kv**

🌐 [**https://share106-1.utsa.tech/celestia/**](https://share106-1.utsa.tech/celestia/)
{% endhint %}

```shell
cd $HOME
systemctl stop celestia-appd

cp $HOME/.celestia-app-main/data/priv_validator_state.json $HOME/.celestia-app-main/priv_validator_state.json.backup

rm -rf $HOME/.celestia-app-main/data 

curl -o - -L https://share106-1.utsa.tech/celestia/celestia_archive_mainnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.celestia-app-main/

mv $HOME/.celestia-app-main/priv_validator_state.json.backup $HOME/.celestia-app-main/data/priv_validator_state.json

systemctl restart celestia-appd && journalctl -u celestia-appd -f -o cat
```



## Bridge Node

{% hint style="success" %}
### Snapshot Archive

every 7 day **|** /root/.celestia-bridge-mocha-4 **|** archive

🌐 [**https://share106-1.utsa.tech/celestia/**](https://share106-1.utsa.tech/celestia/)
{% endhint %}

```shell
cd $HOME
systemctl stop celestia-bridge

rm -rf /root/.celestia-bridge/{blocks,data,.lock}

wget https://share106-2.utsa.tech/celestia/celestia_bridge_mainnet.tar.lz4
lz4 -dc celestia_bridge_mainnet.tar.lz4 | tar -xvf - -C /root/.celestia-bridge/

systemctl restart celestia-bridge && journalctl -u celestia-bridge -f -o cat
```



## Full Node

{% hint style="success" %}
### Snapshot

every 30 hours **|** /root/.celestia-full **|** catchup headers: true

🌐 [**https://share106-6.utsa.tech/celestia/**](https://share106-6.utsa.tech/celestia/)
{% endhint %}

<pre class="language-shell"><code class="lang-shell">cd $HOME
systemctl stop celestia-full

rm -rf /root/.celestia-full/{blocks,data,.lock}

wget https://share106-6.utsa.tech/celestia/celestia_full_mainnet.tar.lz4
lz4 -dc celestia_full_mainnet.tar.lz4 - | tar -xvf - -C /root/.celestia-full/
<strong>
</strong><strong>systemctl restart celestia-full &#x26;&#x26; journalctl -u celestia-full -f -o cat
</strong></code></pre>



