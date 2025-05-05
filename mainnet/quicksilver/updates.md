# 📬 Updates

##

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

## UPD 🕊 on v1.2.0 !!!NETWORK STARTS FROM ZERO!!!

```shell
systemctl stop quicksilverd
cd $HOME/quicksilver
git fetch && git checkout v1.2.0
#HEAD is now at 0ce6daf Release/v1.2.0 (#276)
make install

quicksilverd version --long | grep -e version -e commit
# version: v1.2.0
# commit: 0ce6daf33aaeb93e1cb306a1fc8672c0123cffd1

# подготавливаем новый genesis
quicksilverd export --for-zero-height --height 115000 > export-quicksilver-1-115000.json
jq . export-quicksilver-1-115000.json -S -c | shasum -a256
# 7df73ba5fdbaf6f4b5cced3f16b8f44047ad8f42a7a6f87f764413b474e81c54

wget https://raw.githubusercontent.com/ingenuity-build/mainnet/main/migrate-genesis.py
python3 migrate-genesis.py
jq . genesis.json -S -c | shasum -a256
# cab2352d12f9e388bc633d909a26eaea8fc52904990405cd20d72077415a51d2

cp genesis.json ~/.quicksilverd/config/genesis.json
jq . ~/.quicksilverd/config/genesis.json -S -c | shasum -a256
# cab2352d12f9e388bc633d909a26eaea8fc52904990405cd20d72077415a51d2

quicksilverd tendermint unsafe-reset-all
quicksilverd config chain-id quicksilver-2

systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

## UPD 🕊 on v1.2.1  (Update Height: 235000)

```shell
cd $HOME/quicksilver
git pull
git checkout v1.2.1 
make build
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit
# v1.2.1
# 9a3355e88738e1f8ff7ff37903abf580d25e5c81

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

## UPD 🕊 on v1.2.2  (Update Height: 248000)

```shell
cd $HOME/quicksilver
git pull
git checkout v1.2.2
make build
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit
# v1.2.2
# d4a45f75c862e217ea7af75db68b9a294fe6831b

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

## UPD 🕊 on v1.2.3  (Update Height: 883000)

```shell
cd $HOME/quicksilver
git pull
git checkout v1.2.3
make build
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit
# v1.2.3
# aa898c9468f542827e9dc9b83fdb02bb416a2b0f

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

## UPD 🕊 on v1.2.4  (unscheduled)

```shell
wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/ingenuity-build/quicksilver/releases/download/v1.2.4/quicksilverd-v1.2.4-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.2.4
#commit: 08236aad4e7f31ab5198be182e4a500473132643
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

## UPD 🕊 on v1.2.7  (Update Height: 1115600)

```shell
wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/ingenuity-build/quicksilver/releases/download/v1.2.7/quicksilverd-v1.2.7-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.2.7
#commit: ce53635a8f372398d2f5f1025cf81d3a5a36f6a8
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

## UPD 🕊 on v1.2.9  (Update Height: 1279200)

```shell
wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/ingenuity-build/quicksilver/releases/download/v1.2.9-hotfix.0/quicksilverd-v1.2.9-hotfix.0-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.2.9-hotfix.0
#commit: 528b54539c89f95a8fdfe5fa70d1878755f83de7
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

## UPD 🕊 on v1.2.11  (Update Height: 1936600)

```shell
wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/ingenuity-build/quicksilver/releases/download/v1.2.11/quicksilverd-v1.2.11-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.2.11
#commit: f27ce70743d14ef694cf8a85b837587dfb9bed5a
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

## UPD 🕊 on v1.2.13  (unscheduled)

<pre class="language-shell"><code class="lang-shell">
<strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/ingenuity-build/quicksilver/releases/download/v1.2.13/quicksilverd-v1.2.1-amd64"
</strong>chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.2.13
#commit: 59f536ae51b2b63446ca43a5610442ba19d123c4
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.2.14  (unscheduled)

<pre class="language-shell"><code class="lang-shell"><strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/ingenuity-build/quicksilver/releases/download/v1.2.14/quicksilverd-v1.2.14-amd64"
</strong>chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.2.14
#commit: 363adba3b5f85f8713481a8b8c989402aa262fae
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.2.15  (Update Height: 3052279)

<pre class="language-shell"><code class="lang-shell"><strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/ingenuity-build/quicksilver/releases/download/v1.2.15/quicksilverd-v1.2.15-amd64"
</strong>chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.2.15
#commit: 77265a0dac90ae55b7bf2e5675651a3df38eea5c
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.2.16  (Update Height: )

```shell
wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/ingenuity-build/quicksilver/releases/download/v1.2.16/quicksilverd-v1.2.16-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.2.16
#commit: 4241c477d5464074aa9dd2e55cf7d6ae20463431
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

## UPD 🕊 on v1.2.17  (Update Height: 4530000)

```shell
wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/ingenuity-build/quicksilver/releases/download/v1.2.17/quicksilverd-v1.2.17-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.2.17
#commit: 91a55484e8582906bbacf4ece739e9a03f6d4bd3
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

## UPD 🕊 on v1.4.5  (Update Height: 5432500)

```shell
wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/ingenuity-build/quicksilver/releases/download/v1.4.5/quicksilverd-v1.4.5-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.4.5
#commit: 297ecad54acb4664f8d43d0ae15acbfd72c41b70
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

## UPD 🕊 on v1.4.6  (Update Height: 5493000)

```shell
wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/ingenuity-build/quicksilver/releases/download/v1.4.6/quicksilverd-v1.4.6-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.4.6
#commit: a153a25ba4c98bdbdb3c58b4b34de09901a2adbf
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd && journalctl -u quicksilverd -f -o cat
```

## UPD 🕊 on v1.4.7  (Update Height: 5848000)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong><strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/ingenuity-build/quicksilver/releases/download/v1.4.7/quicksilverd-v1.4.7-amd64"
</strong>chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.4.7
#commit: 356e18826d3c1a84dc88398e8a4a5b6fe45f2a63
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.5.0  (Update Height: 6365700)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.5.0/quicksilverd-v1.5.0-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.5.0
#commit: f3fb2d9b0a156eb01d8c2d410510ad5ae7bc3247
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.5.1  (Update Height: 6452000)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.5.1/quicksilverd-v1.5.1-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.5.1
#commit: ee78617a6fb403baf2cf9c7a951b6a0c7d8be079
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.5.3  (Update Height: 6556300)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.5.3/quicksilverd-v1.5.3-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.5.3
#commit: 02bd08df8cb6a9e2d3bda0923b14bcfb10732c14
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.5.4  (Update Height: 6673000)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.5.4/quicksilverd-v1.5.4-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.5.4
#commit: 7af99332e6625ea1cc640ebabaedaf6fa4ee68e2
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.5.5  (Update Height: 6926000)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.5.5/quicksilverd-v1.5.5-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.5.5
#commit: 33369728097f0606906cd1c0006b3cc88772fa0f
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.5.6  (Update Height: 7810000)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.5.6/quicksilverd-v1.5.6-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.5.6
#commit: 564d5081e4ce6a72d04237d481070812259df0bc
#build_tags: netgo ledger muslc,

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.6.1  (Update Height: 8003500)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.6.1-hf/quicksilverd-v1.6.1-hf-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.6.1-hf
#commit: 401b3a8e4bc544debf1139f7117e7e311d120806

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.6.2  (Update Height: 8222500)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.6.2/quicksilverd-v1.6.2-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.6.2
#commit: de7a37cd29925b977bddaff91b60254ad3cfcc9e

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.6.3  (Update Height: 8722500)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.6.3/quicksilverd-v1.6.3-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.6.3
#commit: 965a6c898be7d5d7a8e19b9c4e1984f3ce58f722

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.6.4  (Update Height: 9829000)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.6.4/quicksilverd-v1.6.4-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.6.4
#commit: 4c015b54959be330149920629adc0707f08830b5

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.7.3  (Update Height: 10143500)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.7.3/quicksilverd-v1.7.3-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.7.3
#commit: f41e83cf2aa5930fbcac8515456f846df59b44ee

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.7.4  (Update Height: 10245100)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.7.4/quicksilverd-v1.7.4-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.7.4
#commit: c9ecfb340f4a95994efc8b6b84eb0df5edf950fc

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.7.5  (Update Height: 10350200)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.7.5/quicksilverd-v1.7.5-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.7.5
#commit: 

# ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 644d7fd9214c824fb85f37fb9a9bf7d2a4eb5816

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.7.6  (Update Height: 11397000)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.7.6/quicksilverd-v1.7.6-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.7.6
#commit: 3ba2de7b55622eaec99792e1ef4f97fcd70e801b

# ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.7.7  (Update Height: 11490000)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.7.7/quicksilverd-v1.7.7-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.7.7
#commit: bd187e1a9e75b397877f75ea9d3e107eb76c2dae

# ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>

## UPD 🕊 on v1.8.0  (Update Height: 12571257)

<pre class="language-shell"><code class="lang-shell"><strong>
</strong>wget -O $HOME/quicksilver/build/quicksilverd "https://github.com/quicksilver-zone/quicksilver/releases/download/v1.8.0/quicksilverd-v1.8.0-amd64"
chmod +x $HOME/quicksilver/build/quicksilverd
$HOME/quicksilver/build/quicksilverd version --long | grep -e version -e commit -e build_tags
#version: v1.8.0
#commit: 4b85b622506c4e145932df04f80ee1c359dd0926

# ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop quicksilverd
mv $HOME/quicksilver/build/quicksilverd $(which quicksilverd)
quicksilverd version --long | grep -e version -e commit
# 

systemctl restart quicksilverd &#x26;&#x26; journalctl -u quicksilverd -f -o cat
</code></pre>
