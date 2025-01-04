# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.dymension

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}



## UPD 🕊 on v2.0.0-alpha.3 (Update Height: 1651535)

```shell
cd $HOME/dymension
git pull
git checkout v2.0.0-alpha.7
make build
$HOME/dymension/build/dymd version --long | grep -e version -e commit
# version: v2.0.0-alpha.7
# commit: 4b6f8d13e553d6316a3d1978f939848068042976

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop dymd
mv $HOME/dymension/build/dymd $(which dymd)
dymd version --long | grep -e version -e commit
# 

systemctl restart dymd && journalctl -u dymd -f -o cat
```

## UPD 🕊 on v2.0.0-alpha.8 (Update Height: 2079000)

```shell
cd $HOME/dymension
git pull
git checkout v2.0.0-alpha.8
make build
$HOME/dymension/build/dymd version --long | grep -e version -e commit
# version: v2.0.0-alpha.8
# commit: 080bbb9ede90170f84a3f4f3665c179ea8742716

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop dymd
mv $HOME/dymension/build/dymd $(which dymd)
dymd version --long | grep -e version -e commit
# 

systemctl restart dymd && journalctl -u dymd -f -o cat
```
