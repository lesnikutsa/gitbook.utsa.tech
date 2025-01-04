# ⌚ Snapshot

## Auto snapshot

```bash
wget -O autosnap_story.sh https://raw.githubusercontent.com/lesnikutsa/story/refs/heads/main/autosnap_story.sh && chmod +x autosnap_story.sh && ./autosnap_story.sh
```



## Manual snapshot

{% hint style="warning" %}
Be careful with snapshot during network outages. If configured incorrectly, you can get a double signature
{% endhint %}

{% hint style="success" %}
### Snapshot Archive

type: **archive |** time: **every 3 days |** pruning: **nothing |** indexer: **kv**

🌐 [**https://share102.utsa.tech/story/**](https://share102.utsa.tech/story/)
{% endhint %}

```shell
systemctl stop story
systemctl stop story-geth

cp $HOME/.story/story/data/priv_validator_state.json $HOME/.story/story/priv_validator_state.json.backup

rm -rf $HOME/.story/story/data
rm -rf $HOME/.story/geth/odyssey/geth/chaindata

# story
curl -o - -L https://share102.utsa.tech/story/story_testnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.story/story/

# story_geth
curl -o - -L https://share102.utsa.tech/story/story_geth_testnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.story/geth/odyssey/geth/

mv $HOME/.story/story/priv_validator_state.json.backup $HOME/.story/story/data/priv_validator_state.json

systemctl restart story
systemctl restart story-geth
```

{% hint style="success" %}
### Snapshot Pruned

type: **pruned |** time: **every 3 hours |** pruning: **everything |** indexer: **null**

🌐 [**https://share106-7.utsa.tech/story/**](https://share106-7.utsa.tech/story/)
{% endhint %}

```shell
systemctl stop story
systemctl stop story-geth

cp $HOME/.story/story/data/priv_validator_state.json $HOME/.story/story/priv_validator_state.json.backup

rm -rf $HOME/.story/story/data
rm -rf $HOME/.story/geth/odyssey/geth/chaindata

# story
curl -o - -L https://share106-7.utsa.tech/story/story_testnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.story/story/

# story_geth
curl -o - -L https://share106-7.utsa.tech/story/story_geth_testnet.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.story/geth/odyssey/geth/

mv $HOME/.story/story/priv_validator_state.json.backup $HOME/.story/story/data/priv_validator_state.json

systemctl restart story
systemctl restart story-geth
```
