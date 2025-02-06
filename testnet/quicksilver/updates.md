# 📬 Updates



{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.quicksilverd

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
<mark style="color:green;">**With full synchronization, start from**</mark>**&#x20;1.1.0-innuendo**
{% endhint %}



## UPD 🕊 on  v1.7.1 (before **573000**)

<pre class="language-sh"><code class="lang-sh"><strong>cd $HOME/quicksilver
</strong>wget -O quicksilverd https://github.com/quicksilver-zone/quicksilver/releases/download/v1.7.1/quicksilverd-v1.7.1-amd64
chmod +x $HOME/quicksilver/quicksilverd
<strong>$HOME/quicksilver/quicksilverd version --long | grep -e version -e commit -e build_tags
</strong># version: v1.7.1
# build_tags: netgo ledger muslc,
# commit: f11e862d6c39e8b355c7c4fa55da18ce36357752

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop quicksilverd
</strong>mv $HOME/quicksilver/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit -e build_tags
#

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on  v1.7.6-testnet (before 1863000)

<pre class="language-sh"><code class="lang-sh"><strong>cd $HOME/quicksilver
</strong>wget -O quicksilverd https://github.com/quicksilver-zone/quicksilver/releases/download/v1.7.6-testnet/quicksilverd-v1.7.6-testnet-amd64
chmod +x $HOME/quicksilver/quicksilverd
<strong>$HOME/quicksilver/quicksilverd version --long | grep -e version -e commit -e build_tags
</strong># version: v1.7.6-testnet
# build_tags: netgo ledger muslc,
# commit: 402cb0b2974f0fd1ec4e5a071b6e85122ca598d2

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop quicksilverd
</strong>mv $HOME/quicksilver/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit -e build_tags
#

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>
