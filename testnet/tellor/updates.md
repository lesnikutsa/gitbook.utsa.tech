# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.layer

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}



## UPD 🕊 on v3.0.1 (Update Height: 1185570)

```shell
cd $HOME/layer
git pull
git checkout v3.0.4
make build
$HOME/layer/build/layerd version --long | grep -e version -e commit
# version: v3.0.4
# commit: 0f1046a0d35df404c01dcb29b09b275301982f45

# # ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop layerd
mv $HOME/layer/build/layerd $(which layerd)
layerd version --long | grep -e version -e commit
#

systemctl restart layerd && journalctl -u layerd -f -o cat
```

## UPD 🕊 on v3.0.4 (Update Height: 2154000)

```shell
cd
rm -r $HOME/layer
git clone https://github.com/tellor-io/layer && cd layer

wget https://github.com/tellor-io/layer/releases/download/v4.0.3/layer_Linux_x86_64.tar.gz
tar -xvzf layer_Linux_x86_64.tar.gz
$HOME/layer/layerd version --long | grep -e version -e commit
# version: 4.0.3
# commit: 683b709191e1342b8722f62b0f9bb897b525a92f

# # ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop layerd
mv $HOME/layer/build/layerd $(which layerd)
layerd version --long | grep -e version -e commit
#

systemctl restart layerd && journalctl -u layerd -f -o cat
```
