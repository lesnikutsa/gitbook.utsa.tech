# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.luggom

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on testnet-2026-10-29 (Update Height: 140000)

```shell
cd $HOME/Luggom
mkdir -p $HOME/Luggom/build

git fetch --all --tags
git checkout testnet-2026-10-29
GOBIN="$HOME/Luggom/build" make install

$HOME/Luggom/build/luggomd version --long | grep -e version -e commit
# version: testnet-2026-10-29
# commit: 985e586341cc81ea5d267f0ef8c0d48e43f5204a

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop luggomd
mv $HOME/Luggom/build/luggomd $HOME/go/bin/luggomd
luggomd version --long | grep -e version -e commit
#

systemctl restart luggomd && journalctl -u luggomd -f -o cat
```
