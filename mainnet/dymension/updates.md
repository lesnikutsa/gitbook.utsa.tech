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



## UPD 🕊 on v3.1.0 (Update Height: 1159000)

```shell
cd $HOME/dymension
git pull
git checkout v3.1.0
make build
$HOME/dymension/build/dymd version --long | grep -e version -e commit
# version: b0e997d6b921666b7c4da4e121a87a2652ed0d2a
# commit: v3.1.0

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop dymd
mv $HOME/dymension/build/dymd $(which dymd)
dymd version --long | grep -e version -e commit
# 

systemctl restart dymd && journalctl -u dymd -f -o cat
```

## UPD 🕊 on v3.2.0 (Update Height: 5135550)

```shell
cd $HOME/dymension
git pull
git checkout v3.2.0
make build
$HOME/dymension/build/dymd version --long | grep -e version -e commit
# version: b77a9e11a4e890135274effd48e318c17e231e25
# commit: v3.2.0

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop dymd
mv $HOME/dymension/build/dymd $(which dymd)
dymd version --long | grep -e version -e commit
# 

systemctl restart dymd && journalctl -u dymd -f -o cat
```
