# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.aura

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```

{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

{% hint style="info" %}
<mark style="color:green;">**With full synchronization, start from**</mark>**&#x20;aura\_v0.4.4**
{% endhint %}

## UPD 🕊 on  v0.8.3 (Update Height: 6644150)

```shell
cd $HOME/aura
git pull
git checkout v0.8.3
make build
$HOME/aura/build/aurad version --long | grep -e version -e commit -e build
# version: v0.8.3
# commit: 057181703213bf089da0525075db25841e00ba94
# build_tags: ""

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop aurad
mv $HOME/aura/build/aurad $(which aurad)
aurad version --long | grep -e version -e commit
#

rm -r $HOME/.aura/wasm/wasm/cache

systemctl restart aurad && journalctl -u aurad -f -o cat
```

## UPD 🕊 on  v0.9.3 (Update Height: 7810560)

```shell
cd $HOME/aura
git pull
git checkout v0.9.3
make build
$HOME/aura/build/aurad version --long | grep -e version -e commit -e build
# version: v0.9.3
# commit: ae45a4ef71da0e09543a926825662c555af507bd
# build_tags: ""

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop aurad
mv $HOME/aura/build/aurad $(which aurad)
aurad version --long | grep -e version -e commit
#

rm -r $HOME/.aura/wasm/wasm/cache

systemctl restart aurad && journalctl -u aurad -f -o cat
```
