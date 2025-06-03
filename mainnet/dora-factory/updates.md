# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.dora

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```

{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}



## UPD 🕊 on 0.4.1 (Update Height: 6308000)

```shell
cd
rm -rf doravota
git clone https://github.com/DoraFactory/doravota && cd doravota
git checkout 0.4.1
make build
$HOME/doravota/build/dorad version --long
# version: 0.4.1
# commit: ""

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop dorad
mv $HOME/doravota/build/dorad $(which dorad)
dorad version --long | grep -e version -e commit
# 

systemctl restart dorad && journalctl -u dorad -f -o cat
```

## UPD 🕊 on 0.4.2 (Update Height: 8818888)

```shell
cd
rm -rf doravota
git clone https://github.com/DoraFactory/doravota && cd doravota
git checkout 0.4.2
make build
$HOME/doravota/build/dorad version --long
# version: 0.4.2
# commit: ""

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop dorad
mv $HOME/doravota/build/dorad $(which dorad)
dorad version --long | grep -e version -e commit
# 

systemctl restart dorad && journalctl -u dorad -f -o cat
```

## UPD 🕊 on 0.4.3 (Update Height: 10200000)

```shell
cd
rm -rf doravota
git clone https://github.com/DoraFactory/doravota && cd doravota
git checkout 0.4.3
make build
$HOME/doravota/build/dorad version --long
# version: 0.4.3
# commit: ""

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop dorad
mv $HOME/doravota/build/dorad $(which dorad)
dorad version --long | grep -e version -e commit
# 

systemctl restart dorad && journalctl -u dorad -f -o cat
```
