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

## UPD 🕊 on v0.6.1 (Update Height: 1968800)

```shell
cd $HOME/wardenprotocol
wget -O $HOME/wardenprotocol/wardend "https://github.com/warden-protocol/wardenprotocol/releases/download/v0.6.1/wardend-0.6.1-linux-amd64"
chmod +x $HOME/wardenprotocol/wardend
$HOME/wardenprotocol/wardend version --long | grep -e version -e commit
# version: 0.6.1
# commit: c0401ff5c3373b4e0a91a3ff7698c97ab2fc30f7

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!
systemctl stop wardend
mv $HOME/wardenprotocol/wardend $(which wardend)
wardend version --long | grep -e version -e commit
# version:
```
