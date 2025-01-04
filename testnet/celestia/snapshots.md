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
systemctl stop celestia-appd-test

cp $HOME/.celestia-app-test/data/priv_validator_state.json $HOME/.celestia-app-test/priv_validator_state.json.backup

rm -rf $HOME/.celestia-app-test/data 

curl -o - -L https://share106-3.utsa.tech/celestia/celestia_pruned_testnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.celestia-app-test/

mv $HOME/.celestia-app-test/priv_validator_state.json.backup $HOME/.celestia-app-test/data/priv_validator_state.json

systemctl restart celestia-appd-test && journalctl -u celestia-appd-test -f -o cat
```

{% hint style="success" %}
### Snapshot Archive

time: **every 3 days** **|** pruning: **nothing |** indexer: **kv**

🌐 [**https://share106-4.utsa.tech/celestia/**](https://share106-4.utsa.tech/celestia/)
{% endhint %}

```shell
cd $HOME
systemctl stop celestia-appd-test

cp $HOME/.celestia-app-test/data/priv_validator_state.json $HOME/.celestia-app-test/priv_validator_state.json.backup

# удаляем старую базу данных
rm -rf $HOME/.celestia-app-test/data 

# скачиваем snapshot
curl -o - -L https://share106-4.utsa.tech/celestia/celestia_archive_testnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.celestia-app-test/

mv $HOME/.celestia-app-test/priv_validator_state.json.backup $HOME/.celestia-app-test/data/priv_validator_state.json

systemctl restart celestia-appd-test && journalctl -u celestia-appd-test -f -o cat
```



## Bridge Node

{% hint style="warning" %}
**IMPORTANT:** unpacking may take more than 24 hours, so run unpacking in tmux or screen
{% endhint %}

{% hint style="success" %}
### Snapshot Archive

every 3 days **|** /root/.celestia-bridge-mocha-4 **|** archival

🌐 [**https://share106-1.utsa.tech/celestia/**](https://share106-1.utsa.tech/celestia/)
{% endhint %}

```shell
cd $HOME
systemctl stop celestia-bridge

rm -rf /root/.celestia-bridge-mocha-4/{blocks,data,.lock}

wget https://share106-1.utsa.tech/celestia/celestia_bridge_testnet.tar.lz4
lz4 -dc celestia_bridge_testnet.tar.lz4 - | tar -xvf - -C /root/.celestia-bridge-mocha-4/

systemctl restart celestia-bridge && journalctl -u celestia-bridge -f -o cat
```



## Full Node

{% hint style="success" %}
### Snapshot

every 48 hours **|** /root/.celestia-full-mocha-4 **|** archival

🌐 [**https://share106-5.utsa.tech/celestia/**](https://share106-5.utsa.tech/celestia/)
{% endhint %}

```shell
cd $HOME
systemctl stop celestia-full

rm -rf /root/.celestia-full-mocha-4/{blocks,data,.lock}

wget https://share106-5.utsa.tech/celestia/celestia_full_testnet.tar.lz4
lz4 -dc celestia_full_testnet.tar.lz4 - | tar -xvf - -C /root/.celestia-full-mocha-4/

systemctl restart celestia-full && journalctl -u celestia-full -f -o cat
```



## Light Node

{% hint style="success" %}
### Snapshot

every 1 day **|** /root/.celestia-light-mocha-4

🌐 [**https://share106-3.utsa.tech/celestia/**](https://share106-3.utsa.tech/celestia/)
{% endhint %}

```shell
cd $HOME
systemctl stop celestia-light

rm -rf ~/.celestia-light-mocha-4/{data,.lock}

curl -o - -L https://share106-3.utsa.tech/celestia/celestia_light_testnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.celestia-light-mocha-4/

systemctl restart celestia-light && journalctl -u celestia-light -f -o cat
```

