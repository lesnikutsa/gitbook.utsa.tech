# ⌚ State sync

{% hint style="info" %}
State sync allows a new node to join a network by fetching a snapshot of the network state at a recent height, instead of fetching and replaying all historical blocks
{% endhint %}

{% hint style="info" %}
Some blockchains do not yet know how to transfer wasm along with State sync. Download wasm if necessary
{% endhint %}

{% hint style="info" %}
Important - different blockchains need a different amount of RAM to successfully start with State sync. Average 4 GB RAM
{% endhint %}

## State sync

<mark style="color:red;">Statesync does not work with this version of the binary</mark>

```shell
# stop the service and clear the database
systemctl stop lavad
lavad tendermint unsafe-reset-all --home $HOME/.lava --keep-addr-book
```

```shell
# add peer
peers="1d3093428c68ee5aeeaa4ebd91e1ac4b69c218c7@144.76.29.90:61356"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.lava/config/config.toml
```

```shell
SNAP_RPC=https://m-lava.rpc.utsa.tech:443

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 100)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.lava/config/config.toml
```

{% hint style="info" %}
after echo <mark style="color:blue;">$LATEST\_HEIGHT $BLOCK\_HEIGHT $TRUST\_HASH</mark> you should see something like this

![](<../../.gitbook/assets/image (29).png>)
{% endhint %}

```shell
# restart node
systemctl restart lavad && journalctl -u lavad -f -o cat
```

{% hint style="warning" %}
Attention. You may see the following errors **ERR snapshot manager not configured ERR State sync failed err="state sync aborted" module=statesync**

In this case, set the value of snapshot-interval to be greater than zero
{% endhint %}

