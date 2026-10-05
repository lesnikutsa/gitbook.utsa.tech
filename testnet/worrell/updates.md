# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.worrell

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on v0.1.3 (Update Height: 1186000)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/worrell
</strong>mkdir -p $HOME/worrell/build
git pull
git checkout v0.1.3
GOTOOLCHAIN=go1.26.5 GOBIN=$HOME/worrell/build make install

$HOME/worrell/build/worrelld version --long | grep -e version -e commit
# version: v0.1.3
# commit: a914f444004df7514ee909c4fa2e66a942059ca6

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop worrelld
mv $HOME/worrell/build/worrelld $HOME/go/bin/worrelld
worrelld version --long | grep -e version -e commit
#

systemctl restart worrelld &#x26;&#x26; journalctl -u worrelld -f -o cat
</code></pre>
