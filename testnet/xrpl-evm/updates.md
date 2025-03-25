# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.exrpd

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on v7.0.0 (Update Height: )

```shell
cd $HOME/xrpl
git pull
git checkout v7.0.0
make build
$HOME/xrpl/bin/exrpd version --long | grep -e version -e commit
# version: v7.0.0
# commit: 9f634ac204f3fdd882cdc06c33cdf6b245a0e358


# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!
systemctl stop exrpd
mv $HOME/xrpl/bin/exrpd $(which exrpd)
exrpd version --long | grep -e version -e commit
#

systemctl restart exrpd && journalctl -u exrpd -f -o cat
```
