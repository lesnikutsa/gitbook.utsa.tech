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

## UPD 🕊 on v (Update Height: )

```shell
cd $HOME/mucoin
git pull
git checkout v
make build
$HOME/mucoin/build/mucoind version --long | grep -e version -e commit
# 
# commit: 


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop mucoind
mv $HOME/XXX $(which mucoind)
mucoind version --long | grep -e version -e commit
# 

systemctl restart mucoind && journalctl -u mucoind -f -o cat
```

##
