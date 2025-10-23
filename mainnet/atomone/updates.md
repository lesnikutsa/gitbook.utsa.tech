# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.atomone

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```

{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

{% hint style="info" %}
<mark style="color:green;">**With full synchronization, start from**</mark>**&#x20;aura\_v0.4.4**
{% endhint %}

## UPD 🕊 on  v1.1.2 (Update Height: )

```shell
cd $HOME/atomone
git pull
git checkout v1.1.2
make build
$HOME/atomone/build/atomoned version --long | grep -e version -e commit
# version: v1.1.2
# commit: b07127b42c91c8709a8b7335ae8a1176e0fd1ece

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop atomoned
mv $HOME/atomone/build/atomoned $(which atomoned)
atomoned version --long | grep -e version -e commit
#

systemctl restart atomoned && journalctl -u atomoned -f -o cat
```

## UPD 🕊 on  v2.1.0 (Update Height: 3318000)

```shell
cd $HOME/atomone
git pull
git checkout v2.1.0
make build
$HOME/atomone/build/atomoned version --long | grep -e version -e commit
# version: v2.1.0
# commit: c29e6ecf39ce733fe14e7d703aa28e5aeef5e8f0

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop atomoned
mv $HOME/atomone/build/atomoned $(which atomoned)
atomoned version --long | grep -e version -e commit
#

systemctl restart atomoned && journalctl -u atomoned -f -o cat
```

## UPD 🕊 on  v3.0.3 (Update Height: 5902000)

```shell
cd $HOME/atomone
git pull
git checkout v3.0.3
make build
$HOME/atomone/build/atomoned version --long | grep -e version -e commit
# version: v3.0.3
# commit: a948052ffbe9b594070abe601b39f91ac1a8498c

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop atomoned
mv $HOME/atomone/build/atomoned $(which atomoned)
atomoned version --long | grep -e version -e commit
#

systemctl restart atomoned && journalctl -u atomoned -f -o cat
```
