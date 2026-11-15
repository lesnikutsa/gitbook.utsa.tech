# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.atlas

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
<mark style="color:green;">**With full synchronization, start from**</mark> v1.0.0
{% endhint %}

## UPD 🕊 on  v (Update Height: )

```shell
cd $HOME/atlas
mkdir -p $HOME/atlas/build
git pull
git checkout v
GOTOOLCHAIN=go1.26.5 GOBIN=$HOME/atlas/build make install

$HOME/atlas/build/atlasd version --long | grep -e version -e commit
# version: 
# commit: 

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop atlasd
mv $HOME/atlas/build/atlasd $HOME/go/bin/atlasd
atlasd version --long | grep -e version -e commit
#

systemctl restart atlasd && journalctl -u atlasd -f -o cat
```

