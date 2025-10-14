# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.arkeo

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on v1.0.11 (Update Height: 252000)

```shell
cd $HOME/arkeo
git pull
git checkout v1.0.11
make install
arkeod version --long | grep -e version -e commit
# version: 
# commit: 45599cd830e353a788e991d51dbabc84fd77ea4f

# AFTER STOPPING THE NETWORK ON THE NECESSARY BLOCK!!!
systemctl stop arkeod

systemctl restart arkeod && journalctl -u arkeod -f -o cat
```

## UPD 🕊 on v1.0.13 (Update Height: 483000)

```shell
cd $HOME/arkeo
git pull
git checkout v1.0.13
make build
arkeod version --long | grep -e version -e commit
# version: v1.0.13
# commit: 452e2688ae9fb343daabeb4986f220164f0f5401

# AFTER STOPPING THE NETWORK ON THE NECESSARY BLOCK!!!
systemctl stop arkeod

systemctl restart arkeod && journalctl -u arkeod -f -o cat
```

## UPD 🕊 on v1.0.14 (Update Height: 1526000)

```shell
cd $HOME/arkeo
git pull
git checkout v1.0.14
make build
arkeod version --long | grep -e version -e commit
# version: v1.0.14
# commit: 

# AFTER STOPPING THE NETWORK ON THE NECESSARY BLOCK!!!
systemctl stop arkeod

systemctl restart arkeod && journalctl -u arkeod -f -o cat
```

## UPD 🕊 on v1.0.15 (Update Height: **2926500**)

```shell
cd $HOME/arkeo
git pull
git checkout v1.0.15
make build
arkeod version --long | grep -e version -e commit
# version: v1.0.15
# commit: b9ccadb59ba761b1a2c3497cae141aa7bdacd7dc

# AFTER STOPPING THE NETWORK ON THE NECESSARY BLOCK!!!
systemctl stop arkeod

systemctl restart arkeod && journalctl -u arkeod -f -o cat
```
