# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.axoned

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}



## UPD 🕊 on  (Update Height: )

```shell
cd $HOME/axoned
git pull
git checkout v5.0.0
make build
$HOME/axoned/target/dist/axoned version --long | grep -e version -e commit
#version: 
#commit: 

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop axoned
mv $HOME/axoned/target/dist/axoned $(which axoned)

axoned version --long | grep -e version -e commit
# 

systemctl restart axoned && journalctl -u axoned -f -o cat
```
