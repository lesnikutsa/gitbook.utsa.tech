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

## UPD 🕊 on v (Update Height: )

```shell
cd $HOME/xrpl
git pull
git checkout v
make build
$HOME/xrpl/build/exrpd version --long | grep -e version -e commit
# version: v
# commit: 


# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!
systemctl stop exrpd
mv $HOME/xrpl/build/exrpd $(which exrpd)
exrpd version --long | grep -e version -e commit
#

systemctl restart exrpd && journalctl -u exrpd -f -o cat
```
