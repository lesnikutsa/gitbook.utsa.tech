# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.entrypoint

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on v1.2.0 (Update Height: 351000)

```shell
mkdir -p $HOME/entrypoint/build
cd $HOME/entrypoint/build

wget -O entrypointd "https://github.com/entrypoint-zone/testnets/releases/download/v1.2.0/entrypointd-1.2.0-linux-amd64"
chmod 744 entrypointd
$HOME/entrypoint/build/entrypointd version --long | grep -e version -e commit -e build
# version: 1.2.0
# commit: 296eee5efbc6c41688795333886febb67a9eeb65
# build_tags: ""

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop entrypointd
mv $HOME/entrypoint/build/entrypointd $(which entrypointd)
entrypointd version --long | grep -e version -e commit -e build
#

systemctl restart entrypointd && journalctl -u entrypointd -f -o cat
```
