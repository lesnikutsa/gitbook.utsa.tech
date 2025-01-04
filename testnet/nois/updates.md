# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.noisd

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}



## UPD 🕊 on v1.0.2  (Update Height: unscheduled)

```shell
cd $HOME/noisd
git pull
git checkout v1.0.2
make install
noisd version --long |  grep -e version -e commit -e build
# version: 1.0.2
# commit: 524c5393ec8ac3753b4da4f22b37b0d75b947f6d
# build_tags: netgo,ledger
noisd query wasm libwasmvm-version
# 1.2.3

systemctl stop noisd
rm -r ~/.noisd/wasm/wasm/cache
systemctl restart noisd && journalctl -u noisd -f -o cat

```

## UPD 🕊 on v1.0.3 (Update Height: unscheduled)

```shell
cd $HOME/noisd
git pull
git checkout v1.0.3
make build
$HOME/noisd/build/noisd version --long | grep -e version -e commit -e build
# version: v1.0.3
# commit: 1a805a0f89f1624cf9428ac2a9ffc1d2a9e1ac97
# build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop noisd
mv $HOME/noisd/build/noisd $(which noisd)
noisd version --long | grep -e version -e commit
# 

rm -r ~/.noisd/wasm/wasm/cache
systemctl restart noisd && journalctl -u noisd -f -o cat
```

## UPD 🕊 on v1.0.4 (Update Height: unscheduled)

```shell
cd $HOME/noisd
git pull
git checkout v1.0.4
make build
$HOME/noisd/build/noisd version --long | grep -e version -e commit -e build
# version: 1.0.4
# commit: 1d5905dbbe6fcf757026abe8110786fd9746c1ec
# build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop noisd
mv $HOME/noisd/build/noisd $(which noisd)
noisd version --long | grep -e version -e commit
# 

rm -r ~/.noisd/wasm/wasm/cache
systemctl restart noisd && journalctl -u noisd -f -o cat
```
