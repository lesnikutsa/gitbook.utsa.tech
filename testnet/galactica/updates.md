# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.galactica

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}



## UPD 🕊 on v0.2.4  (Update Height: 2645752)

```shell
cd $HOME/galactica
git pull
git checkout v0.2.4
make build
$HOME/galactica/build/galacticad version --long | grep -e version -e commit
# version: 0.2.4
# commit: c5e0f35dd7f1a040157a96f70b821b6e483f69d4

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop galacticad
mv $HOME/galactica/build/galacticad $(which galacticad)
galacticad version --long | grep -e version -e commit
# 

systemctl restart galacticad && journalctl -u galacticad -f -o cat
```

## UPD 🕊 on v0.2.7  (Update Height: 3475721)

```shell
cd $HOME/galactica
git pull
git checkout v0.2.7
make build
$HOME/galactica/build/galacticad version --long | grep -e version -e commit
# version: 0.2.7
# commit: da411a7a648f2cdd1bf7961b35272218a0c5b92f

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop galacticad
mv $HOME/galactica/build/galacticad $(which galacticad)
galacticad version --long | grep -e version -e commit
# 

systemctl restart galacticad && journalctl -u galacticad -f -o cat
```
