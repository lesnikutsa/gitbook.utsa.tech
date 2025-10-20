# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.canine

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on v1.2.0-alpha.6 (Update Height: 44000)

```shell
cd $HOME/canine-chain
git pull
git checkout v1.2.0-alpha.6
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# 1.2.0-alpha.6
# commit: da3800d7c5b14d8b26e21e24bf2585f68c0577d4

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v1.2.0-alpha.7 (Update Height: 63000)

```shell
cd $HOME/canine-chain
git pull
git checkout v1.2.0-alpha.7
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# v1.2.0-alpha.7
# commit: df0fff1490c0f944ce164045fdbbba8cc51ed5e8

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v1.2.0-alpha.11 (Update Height: 615015)

```shell
cd $HOME/canine-chain
git pull
git checkout v1.2.0-alpha.11
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# 1.2.0-alpha.11
# commit: e8d539251f541944111cd4b040364f1fbd33dc87

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v23.01-beta (Update Height: 853470)

```shell
cd $HOME/canine-chain
git pull
git checkout v23.01-beta
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# v23.01-beta
# commit: ec5e1143a18e0e3392a0a524f83a517c1bf0a76a

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v1.2.0-beta.1 (Update Height: 908770)

```shell
cd $HOME/canine-chain
git pull
git checkout v1.2.0-beta.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
#v1.2.0-beta.1
#commit: 59266fe80695913bf73d973a0488a6c4d66ca143

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v1.2.0-beta.4 (Update Height: 1052500)

```shell
cd $HOME/canine-chain
git pull
git checkout v1.2.0-beta.4
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
#v1.2.0-beta.4
#commit: 4a24de3556e3d6825f143ef249486363ed3d7487

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v1.2.0-beta.5 (Update Height: 1137000)

```shell
cd $HOME/canine-chain
git pull
git checkout v1.2.0-beta.5
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
#v1.2.0-beta.5
#commit: 44380ad7d084ceb94fc218b1e40e9e0586d62f74

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v1.2.0-beta.6 (Update Height: 1162800)

```shell
cd $HOME/canine-chain
git pull
git checkout v1.2.0-beta.6
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
#v1.2.0-beta.6
#commit: 7dc8aabb70364a96a2b99e8dc66903045db38ec4

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v1.2.0-beta.7 (Update Height: 1506700)

```shell
cd $HOME/canine-chain
git pull
git checkout v1.2.0-beta.7
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
#v1.2.0-beta.7
#commit: be7f28a1f76da0960ecb1a9f020f22dc8c67fe4c

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v1.2.2-alpha.1 (Update Height: 1652440)

```shell
cd $HOME/canine-chain
git pull
git checkout v1.2.2-alpha.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#v1.2.2-alpha.1
#commit: 653ff00299c7fdc6e46b84e1332b0003e9f23ac3
#build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v2.0.0-rc.0 (Update Height: 2216925)

```shell
cd $HOME/canine-chain
git pull
git checkout v2.0.0-rc.0
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: canary
#commit: 25982076fa5ebf5b9867cf5c67721284496ae2aa
#build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v2.1.0-rc.2 (Update Height: 3313000)

```shell
cd $HOME/canine-chain
git pull
git checkout v2.1.0-rc.2
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: 2.1.0-rc.2
#commit: 18b171189b5fba890ca861a08e8774400e346a86
#build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v3.0.0-rc.2 (Update Height: 3654000)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/canine-chain
</strong>git pull
git checkout v3.0.0-rc.2
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: 3.0.0-rc.2
#commit: f537ff0dcde3177da7b6e18ccb9608ad5b471412
#build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v3.0.2-rc.1 (Update Height: )

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/canine-chain
</strong>git pull
git checkout v3.0.0-rc.3
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: 3.0.2-rc.1
#commit: 4c745ddb7168d6e869d987099e233eb44396e494
#build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v3.1.0-rc.2 (Update Height: **6133525**)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/canine-chain
</strong>git pull
git checkout v3.1.2-rc.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: v3.1.2-rc.1
#commit: 9a88bf707fb3c114edb8f69f6f3f0a2487aba0e6

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v3.2.0-rc.1 (Update Height: **6911060**)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/canine-chain
</strong>git pull
git checkout v3.2.2-beta.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: v3.2.2-beta.1 
#commit: 9fae4023e8e8042620c37630ea8353b173192a57

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v3.4.0-beta.1 (Update Height: 8815100)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/canine-chain
</strong>git pull
git checkout v3.4.0-beta.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: 3.4.0-beta.1
#commit: 399dd63fb26d39b06820fef595d49589c129b869

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v4.0.0-beta.4 (Update Height: 8903800)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/canine-chain
</strong>git pull
git checkout v4.0.0-beta.4
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: 4.0.0-beta.4
#commit: a8f32aea0dd9e2a3499ea3b2ca0ddafd1a53d819

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop canined
</strong>mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v4.0.0-beta.5 (Update Height: 9034000)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/canine-chain
</strong>git pull
git checkout v4.0.0-beta.5
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: 4.0.0-beta.5
#commit: 05785bccff802e963d0c964434d38fc77833d4c3

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop canined
</strong>mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v4.1.0-beta.2 (Update Height: 9923185)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/canine-chain
</strong>git pull
git checkout v4.1.0-beta.2
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: v4.1.0-beta.2
#commit: 4e64e646fa8036eb48e86cb512b0da81bec5e283

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop canined
</strong>mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v4.1.0-rc.1 (Update Height: 10250120)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/canine-chain
</strong>git pull
git checkout v4.1.0-rc.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: v4.1.0-rc.1
#commit: 8b03d850b707ceb88e04f767ac40e7e774065ff8

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop canined
</strong>mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v4.2.0-rc.1 (Update Height: 10468060)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/canine-chain
</strong>git pull
git checkout v4.2.0-rc.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: v4.2.0-rc.1
#commit: 93f0010fde00513bc62586d02d43864cde045f00

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop canined
</strong>mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v4.3.0-rc.1 (Update Height: 11204200)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/canine-chain
</strong>git pull
git checkout v4.3.0-rc.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: v4.3.0-rc.1
#commit: 2572380db56a116b276e18af131752881ed1243d

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop canined
</strong>mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v4.4.0-rc.1 (Update Height: 11744600)

<pre class="language-shell"><code class="lang-shell">cd $HOME/canine-chain
git pull
git checkout v4.4.0-rc.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: v4.4.0-rc.1
#commit: c489b7724f39a3d47087a6ca0af9f6da1bb40f3b

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop canined
</strong>mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v4.5.0-rc.2 (Update Height: 12551640)

<pre class="language-shell"><code class="lang-shell">cd $HOME/canine-chain
git pull
git checkout v4.5.0-rc.2
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: v4.5.0-rc.2
#commit: 35b7b300ef25242b95f47bd1f1b30ee1531dd6de

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop canined
</strong>mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v4.6.0-beta.1 (Update Height: **13222000**)

<pre class="language-shell"><code class="lang-shell">cd
rm -r canine-chain
git clone https://github.com/JackalLabs/canine-chain &#x26;&#x26; cd canine-chain
git checkout v4.6.0-beta.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: canary
#commit: 09d64dcbab0a96da1da1605acfc4914069022ba4

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop canined
</strong>mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v5.0.0-alpha.1 (Update Height: 16081600)

<pre class="language-shell"><code class="lang-shell">cd
rm -r canine-chain
git clone https://github.com/JackalLabs/canine-chain &#x26;&#x26; cd canine-chain
git checkout v5.0.0-alpha.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: 5.0.0-alpha.1
#commit: dcb163514c6e666d0379fa111b3b416ee1604d18

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop canined
</strong>mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>

## UPD 🕊 on v5.0.0-rc.1 (Update Height: 16158160)

<pre class="language-shell"><code class="lang-shell">cd
rm -r canine-chain
git clone https://github.com/JackalLabs/canine-chain &#x26;&#x26; cd canine-chain
git checkout v5.0.0-rc.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit -e build
#version: 5.0.0-rc.1
#commit: cb2e5293e751880fafa6569a8ec8b35e9481b51b

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
<strong>systemctl stop canined
</strong>mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined &#x26;&#x26; journalctl -u canined -f -o cat
</code></pre>
