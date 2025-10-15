# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.provenanced

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on v1.22.0 (Update Height: 23306222)

```shell
export PIO_HOME=~/.provenanced
cd $HOME/provenance
git pull
git checkout v1.22.0
#make golangci-lint

make build
$HOME/provenance/build/provenanced version --long | grep -e version -e commit
# v1.22.0
# commit: 24810d86

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop provenanced
mv $HOME/provenance/build/provenanced $(which provenanced)
provenanced version --long | grep -e version -e commit
# 

systemctl restart provenanced && journalctl -u provenanced -f -o cat
```

## UPD 🕊 on v1.23.0 (Update Height: 23996655)

```shell
export PIO_HOME=~/.provenanced
cd $HOME/provenance
git pull
git checkout v1.23.0
#make golangci-lint

make build
$HOME/provenance/build/provenanced version --long | grep -e version -e commit
# v1.23.0
# commit: 9ffd67f3

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop provenanced
mv $HOME/provenance/build/provenanced $(which provenanced)
provenanced version --long | grep -e version -e commit
# 

systemctl restart provenanced && journalctl -u provenanced -f -o cat
```

## UPD 🕊 on v1.24.0 (Update Height: 24410888)

```
export PIO_HOME=~/.provenanced
cd $HOME/provenance
git pull
git checkout v1.24.0
#make golangci-lint

make build
$HOME/provenance/build/provenanced version --long | grep -e version -e commit
# v1.24.0
# commit: ca136533

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop provenanced
mv $HOME/provenance/build/provenanced $(which provenanced)
provenanced version --long | grep -e version -e commit
# 

systemctl restart provenanced && journalctl -u provenanced -f -o cat
```

## UPD 🕊 on v1.25.1 (Update Height: 25098777)

```
export PIO_HOME=~/.provenanced
cd $HOME/provenance
git pull
git checkout v1.25.1

make build
$HOME/provenance/build/provenanced version --long | grep -e version -e commit
# v1.25.1
# commit: 20e97bf1

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop provenanced
mv $HOME/provenance/build/provenanced $(which provenanced)
provenanced version --long | grep -e version -e commit
# 

systemctl restart provenanced && journalctl -u provenanced -f -o cat
```
