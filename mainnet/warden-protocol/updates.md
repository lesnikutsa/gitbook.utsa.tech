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

## UPD 🕊 on v0.7.2 (Update Height: 745500)

```shell
cd $HOME/wardenprotocol
git pull
git checkout v0.7.2
#just wardend

go build -tags "netgo" -trimpath -ldflags "
    -s -w
    -X github.com/cosmos/cosmos-sdk/version.Name=warden
    -X github.com/cosmos/cosmos-sdk/version.AppName=wardend
    -X github.com/cosmos/cosmos-sdk/version.Version=v0.7.2
    -X github.com/cosmos/cosmos-sdk/version.Commit=b5221f6468410004de503cd5f66273e6a467369a" \
    -o ./build/wardend ./cmd/wardend

$HOME/wardenprotocol/build/wardend version --long | grep -e commit -e version
# version: v0.7.2
# commit: b5221f6468410004de503cd5f66273e6a467369a

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!
systemctl stop wardend
mv $HOME/wardenprotocol/build/wardend $(which wardend)
wardend version --long | grep -e version -e commit
# version: 
systemctl restart wardend && journalctl -u wardend -f -o cat
```





