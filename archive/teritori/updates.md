# 📬 Updates

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.teritorid

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on v1.2.0 (Update Height:)

```shell
cd teritori-chain
git pull
git checkout v1.2.0
make install

teritorid version --long | head
# version: v1.2.0

systemctl restart teritorid && journalctl -u teritorid -f -o cat
```

## UPD 🕊 on v1.3.0 (Update Height:378256)

```shell
cd $HOME/teritori-chain
git pull
git checkout v1.3.0
make build
$HOME/teritori-chain/build/teritorid version --long | grep -e version -e commit
# v1.3.0

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop teritorid
mv $HOME/teritori-chain/build/teritorid $(which teritorid)
teritorid version --long | grep -e version -e commit
# 

systemctl restart teritorid && journalctl -u teritorid -f -o cat
```

## UPD 🕊 on v1.3.1 (Update Height:2147722)

```shell
cd $HOME/teritori-chain
git pull
git checkout v1.3.1
make build
$HOME/teritori-chain/build/teritorid version --long | grep -e version -e commit -e build_tags
# v1.3.1
# commit:13752ae2d11f3b305add3fec90717dab21c60b1c
# build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop teritorid
mv $HOME/teritori-chain/build/teritorid $(which teritorid)
teritorid version --long | grep -e version -e commit
# 

systemctl restart teritorid && journalctl -u teritorid -f -o cat
```

## UPD 🕊 on v1.4.0 (Update Height:3699425)

```shell
cd $HOME/teritori-chain
git pull
git checkout v1.4.0
make build
$HOME/teritori-chain/build/teritorid version --long | grep -e version -e commit -e build_tags
# v1.4.0
# commit:01f60ec7fb9cb9d77dbe7fee1a9d69ff4fb0d2b9
# build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop teritorid
mv $HOME/teritori-chain/build/teritorid $(which teritorid)
teritorid version --long | grep -e version -e commit
# 

systemctl restart teritorid && journalctl -u teritorid -f -o cat
```

## UPD 🕊 on v2.0.6 (Update Height: 7199342)

```shell
cd $HOME/teritori-chain
git pull
git checkout v2.0.6
make build
$HOME/teritori-chain/build/teritorid version --long | grep -e version -e commit -e build_tags
# v2.0.6
# commit:99fa787ee24ccf82c005da8e369e00afd938874a
# build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop teritorid
mv $HOME/teritori-chain/build/teritorid $(which teritorid)
teritorid version --long | grep -e version -e commit
# 

systemctl restart teritorid && journalctl -u teritorid -f -o cat
```
