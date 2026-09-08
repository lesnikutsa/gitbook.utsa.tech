# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.worrell

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
cd $HOME/worrell
git pull
git checkout v0.1.2
make build

$HOME/worrell/build/evmd version --long | grep -e version -e commit
# version: 
# commit: 

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop worrelld
mv $HOME/worrell/build/evmd $HOME/go/bin/worrelld
worrelld version --long | grep -e version -e commit
#

systemctl restart worrelld && journalctl -u worrelld -f -o cat
```
