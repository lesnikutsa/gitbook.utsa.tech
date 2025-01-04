# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.noisd

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}



## UPD 🕊 on v1.0.3 (Update Height: unscheduled)

```shell
cd $HOME/noisd
git pull
git checkout v1.0.5
make build
$HOME/noisd/build/noisd version --long | grep -e version -e commit -e build
# version: v1.0.5
# commit: 1e7b65f785b43e9b389ff7be058d935677fdaf78
# build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop noisd
mv $HOME/noisd/build/noisd $(which noisd)
noisd version --long | grep -e version -e commit
# 

rm -r ~/.noisd/wasm/wasm/cache
systemctl restart noisd && journalctl -u noisd -f -o cat
```
