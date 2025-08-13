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
</strong># version: v1.7.6-testnet2
# build_tags: netgo ledger muslc,
# commit: a1b8617e6c11975a6c8ad5b941a5d066c016704f

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop quicksilverd
</strong>mv $HOME/quicksilver/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit -e build_tags
#

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on  v1.8.0-testnet (before **2786000**)

<pre class="language-sh"><code class="lang-sh"><strong>cd $HOME/quicksilver
</strong>wget -O quicksilverd https://github.com/quicksilver-zone/quicksilver/releases/download/v1.8.0-rc.0/quicksilverd-v1.8.0-rc.0-amd64
chmod +x $HOME/quicksilver/quicksilverd
<strong>$HOME/quicksilver/quicksilverd version --long | grep -e version -e commit -e build_tags
</strong># version: v1.7.0-rc.0
# build_tags: netgo ledger muslc,
# commit: e082f944ba31cc82ab8e857cd69a5fbb91b00910

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop quicksilverd
</strong>mv $HOME/quicksilver/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit -e build_tags
#

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on  v1.8.0-rc.1 (before **2897000**)

<pre class="language-sh"><code class="lang-sh"><strong>cd $HOME/quicksilver
</strong>wget -O quicksilverd https://github.com/quicksilver-zone/quicksilver/releases/download/v1.8.0-rc.1/quicksilverd-v1.8.0-rc.1-amd64
chmod +x $HOME/quicksilver/quicksilverd
<strong>$HOME/quicksilver/quicksilverd version --long | grep -e version -e commit -e build_tags
</strong># version: v1.8.0-rc.1
# build_tags: netgo ledger muslc,
# commit: 044d63e5b802ed1a66c76927e8a760354984cb37

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop quicksilverd
</strong>mv $HOME/quicksilver/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit -e build_tags
#

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on  v1.9.0 (before 4728000)

<pre class="language-sh"><code class="lang-sh"><strong>cd $HOME/quicksilver
</strong>wget -O quicksilverd https://github.com/quicksilver-zone/quicksilver/releases/download/v1.9.0/quicksilverd-v1.9.0-amd64
chmod +x $HOME/quicksilver/quicksilverd
<strong>$HOME/quicksilver/quicksilverd version --long | grep -e version -e commit -e build_tags
</strong># version: v1.9.0
# commit: 07581780639288412daf62dbf2bbaacdccf79ed9

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop quicksilverd
</strong>mv $HOME/quicksilver/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit -e build_tags
#

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

