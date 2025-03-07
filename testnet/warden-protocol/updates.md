# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.warden

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on v0.6.2 (Update Height: 2041100)

```shell
cd $HOME/wardenprotocol
wget -O $HOME/wardenprotocol/wardend "https://github.com/warden-protocol/wardenprotocol/releases/download/v0.6.2/wardend-0.6.2-linux-amd64"
chmod +x $HOME/wardenprotocol/wardend
$HOME/wardenprotocol/wardend version --long | grep -e version -e commit
# version: 0.6.2
# commit: 0f3f752e27e4d86594edfd98a98c12e8eba28568

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!
systemctl stop wardend
mv $HOME/wardenprotocol/wardend $(which wardend)
wardend version --long | grep -e version -e commit
# version:
```
