# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.atomone

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

## UPD 🕊 on  v2.0.0-rc1 (Update Height: 1240000)

```shell
cd $HOME/atomone
git pull
git checkout v2.0.0-rc1
make build
$HOME/atomone/build/atomoned version --long | grep -e version -e commit
# version: v2.0.0-rc1
# commit: aa7aeb7fcbf0c627d53d408ad80562c70c084826

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop atomoned
mv $HOME/atomone/build/atomoned $(which atomoned)
atomoned version --long | grep -e version -e commit
#

systemctl restart atomoned && journalctl -u atomoned -f -o cat
```

