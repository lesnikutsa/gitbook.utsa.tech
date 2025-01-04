# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.empe-chain

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}



## UPD 🕊 on v0.2.2  (Update Height: 1085468)

```shell
cd $HOME/empe-chain
curl -LO https://github.com/empe-io/empe-chain-releases/raw/master/v0.2.2/emped_v0.2.2_linux_amd64.tar.gz
tar -xvf emped_v0.2.2_linux_amd64.tar.gz
rm -r emped_*
chmod 744 $HOME/empe-chain/emped
$HOME/empe-chain/emped version --long | grep -e version -e commit
# version: v0.2.2
# commit: 89a4061fa99a171bb2a15a5c23f0c61749511abd

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop emped
mv $HOME/empe-chain/emped $(which emped)
emped version --long | grep -e version -e commit
# 

systemctl restart emped && journalctl -u emped -f -o cat
```
