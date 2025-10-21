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

## UPD 🕊 on v0.7.0 (Update Height: 1233000)

```shell
cd $HOME/wardenprotocol
git pull
git checkout v0.7.0
#just wardend

go build -tags "netgo" -trimpath -ldflags "
    -s -w
    -X github.com/cosmos/cosmos-sdk/version.Name=warden
    -X github.com/cosmos/cosmos-sdk/version.AppName=wardend
    -X github.com/cosmos/cosmos-sdk/version.Version=v0.7.0
    -X github.com/cosmos/cosmos-sdk/version.Commit=bbb3d8ef4d6041827184a7b5a4d0de7ab6976e18" \
    -o ./build/wardend ./cmd/wardend

$HOME/wardenprotocol/build/wardend version --long | grep -e commit -e version
# version: v0.7.0
# commit: bbb3d8ef4d6041827184a7b5a4d0de7ab6976e18

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!
systemctl stop wardend
mv $HOME/wardenprotocol/build/wardend $(which wardend)
wardend version --long | grep -e version -e commit
# version: 
systemctl restart wardend && journalctl -u wardend -f -o cat
```

## UPD 🕊 on v0.7.1 (Update Height: immediately)

```shell
cd $HOME/wardenprotocol
git pull
git checkout v0.7.1
#just wardend

go build -tags "netgo" -trimpath -ldflags "
    -s -w
    -X github.com/cosmos/cosmos-sdk/version.Name=warden
    -X github.com/cosmos/cosmos-sdk/version.AppName=wardend
    -X github.com/cosmos/cosmos-sdk/version.Version=v0.7.1
    -X github.com/cosmos/cosmos-sdk/version.Commit=028207012cd43509ce9a60e31df22b476b00de1e" \
    -o ./build/wardend ./cmd/wardend

$HOME/wardenprotocol/build/wardend version --long | grep -e commit -e version
# version: v0.7.1
# commit: 028207012cd43509ce9a60e31df22b476b00de1e

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!
systemctl stop wardend
mv $HOME/wardenprotocol/build/wardend $(which wardend)
wardend version --long | grep -e version -e commit
# version: 
systemctl restart wardend && journalctl -u wardend -f -o cat
```
