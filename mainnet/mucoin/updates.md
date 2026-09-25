# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.mucoin

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on rewards-v0.9.0 (Update Height: 771000)

```shell
cd $HOME/mucoin
mkdir -p $HOME/mucoin/build

CGO_ENABLED=0 GOBIN=$HOME/mucoin/build make install
$HOME/mucoin/build/mucoind version --long | grep -e version -e commit
# version: rewards-v0.9.0
# commit: 9c38055e493d2beb0b0bb94e2360b13abd431ae7

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop mucoind
mv $HOME/mucoin/build/mucoind $(which mucoind)
mucoind version --long | grep -e version -e commit
# 

systemctl restart mucoind && journalctl -u mucoind -f -o cat
```

##
