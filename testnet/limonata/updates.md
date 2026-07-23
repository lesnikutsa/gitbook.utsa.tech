# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.evmd

echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter your port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```

{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}



## UPD 🕊 on v limonata-testnet-v0.2.0  (Update Height: 766558)

```shell
cd $HOME/limonata
git pull
git checkout limonata-testnet-v0.2.0
make build

$HOME/limonata/build/evmd version --long | grep -e version -e commit
# version: limonata-testnet-v0.2.0
# commit: c49c08915bd3f8aecce8d874094fff5dcd04ac62

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop limonatad
mv $HOME/limonata/build/evmd $(which limonatad)
limonatad version --long | grep -e version -e commit
#

systemctl restart limonatad && journalctl -u limonatad -f -o cat
```

## UPD 🕊 on limonata-v0.3.4  (Update Height: 998735)

```shell
cd $HOME/limonata
git pull
git checkout limonata-v0.3.4
make build

$HOME/limonata/build/evmd version --long | grep -e version -e commit
# version: limonata-v0.3.4
# commit: 77fc357ffe8d790191e52fefc6a91418e61ef298

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop limonatad
mv $HOME/limonata/build/evmd $HOME/go/bin/limonatad
limonatad version --long | grep -e version -e commit
#

systemctl restart limonatad && journalctl -u limonatad -f -o cat
```

