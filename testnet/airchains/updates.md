# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.junction

echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter your port>

# Checking prevotes/precommits. Useful for updates
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on v0.2.0 (Update Height: 2383911)

```shell
mkdir -p $HOME/airchains && cd airchains
wget -O junctiond https://github.com/airchains-network/junction/releases/download/v0.2.0/junctiond-linux-amd64
chmod +x junctiond
$HOME/airchains/junctiond version --long | grep -e version -e commit
# version: v0.1.0
# commit: e3dd056fd17970e2e3a032afb0f2e293d222f2fe

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop junctiond
mv $HOME/airchains/junctiond $(which junctiond)
junctiond version --long | grep -e version -e commit
# 

systemctl restart junctiond && journalctl -u junctiond -f -o cat
```

