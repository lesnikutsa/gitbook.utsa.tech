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

```shell
# stop the service and clear the database
systemctl stop wardend
wardend tendermint unsafe-reset-all --home $HOME/.warden --keep-addr-book
```

```shell
# add peer
peers="66da788d56b4ad89a4ab84f6c07e377be119a430@144.76.29.90:61256"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.warden/config/config.toml
```

```shell
SNAP_RPC=https://rpc.sentry-1.alfama.wardenprotocol.org:443
# SNAP_RPC=https://t-warden.rpc.utsa.tech:443

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.warden/config/config.toml
```

{% hint style="info" %}
after echo <mark style="color:blue;">$LATEST\_HEIGHT $BLOCK\_HEIGHT $TRUST\_HASH</mark> you should see something like this

![](<../../.gitbook/assets/image (29).png>)
{% endhint %}

```shell
# restart node
systemctl restart wardend && journalctl -u wardend -f -o cat
```



