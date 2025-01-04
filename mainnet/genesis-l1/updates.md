# 📬 Updates

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.genesisd

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on 1.0.0 (Update Height: 7400000)

```shell
cd
rm -r genesisd/
git clone https://github.com/alpha-omega-labs/genesis-crypto && cd genesis-crypto
git checkout v1.0.0
make build
$HOME/genesis-crypto/build/genesisd version --long | grep -e version -e commit
# 1.0.0
# commit: 7cb02ad35442e00b7eb24dc06868577223d48141

# AFTER THE NETWORK STOPS AT THE RIGHT BLOCK!!!
systemctl stop genesisd
mv $HOME/genesis-crypto/build/genesisd $(which genesisd)
genesisd version --long | grep -e version -e commit
# 

systemctl restart genesisd && journalctl -u genesisd -f -o cat
```

##
