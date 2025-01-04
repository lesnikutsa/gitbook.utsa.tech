# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.aura

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD 🕊 on euphoria\_v0.3.0 (Update Height: 702290)

```shell
systemctl stop aurad && cd aura
git pull
git checkout euphoria_v0.3.0
make install
systemctl restart aurad && journalctl -u aurad -f -o cat

aurad version --long | head
# version: euphoria_v0.3.0
# commit: d2585a031a3290cf5c31c016833bbc8fb215f27a
```

## UPD 🕊 on euphoria\_v0.3.1 (Update Height: 876360)

```shell
systemctl stop aurad
cd $HOME && rm -rf aura
git clone https://github.com/aura-nw/aura && cd aura
git checkout euphoria_v0.3.1
make install
systemctl restart aurad && journalctl -u aurad -f -o cat
```

## UPD 🕊 on euphoria\_v0.3.3 (Update Height: 1451700)

```shell
systemctl stop aurad
cd $HOME && rm -rf aura
git clone https://github.com/aura-nw/aura && cd aura
git checkout euphoria_v0.3.3
make install

aurad version --long | head
# euphoria_v0.3.3
# commit: 956f99dc5cc95ce13967f383bf8238a5265d0c43

systemctl restart aurad && journalctl -u aurad -f -o cat
```

## UPD 🕊 on euphoria\_v0.4.2 (Update Height: 2187716)

```shell
cd $HOME/aura
git pull
git checkout euphoria_v0.4.2
make build
$HOME/aura/build/aurad version --long | grep -e version -e commit
# euphoria_v0.4.2
# commit: 7922c61c8c404645007995211070c9d817713c33

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop aurad
mv $HOME/aura/build/aurad $(which aurad)
aurad version --long | grep -e version -e commit
#

systemctl restart aurad && journalctl -u aurad -f -o cat
```

## UPD 🕊 on euphoria\_v0.4.4 (Update Height: 3698067)

```shell
cd $HOME/aura
git pull
git checkout euphoria_v0.4.4
make build
$HOME/aura/build/aurad version --long | grep -e version -e commit
# euphoria_v0.4.4
# commit: b4f8f4807c72c2b8b59d124050322cd6e6a2dd76

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop aurad
mv $HOME/aura/build/aurad $(which aurad)
aurad version --long | grep -e version -e commit
#

systemctl restart aurad && journalctl -u aurad -f -o cat
```

## UPD 🕊 on euphoria\_v0.5.1 (Update Height: 4307903)

```shell
cd $HOME/aura
git pull
git checkout euphoria_v0.5.1
make build
$HOME/aura/build/aurad version --long | grep -e version -e commit
# euphoria_v0.5.1
# commit: 431d35907bd80c3e02bc613ed87c889a9e7eca20

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop aurad
mv $HOME/aura/build/aurad $(which aurad)
aurad version --long | grep -e version -e commit
#

systemctl restart aurad && journalctl -u aurad -f -o cat
```

## UPD 🕊 on v0.6.0 (Update Height: 5449872)

```shell
cd $HOME/aura
git pull
git checkout euphoria_v0.6.0
make build
$HOME/aura/build/aurad version --long | grep -e version -e commit
# euphoria_v0.6.0
# commit: 97bf482285be00b01f3050a41845f3926816d89b

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop aurad
mv $HOME/aura/build/aurad $(which aurad)
aurad version --long | grep -e version -e commit
#

systemctl restart aurad && journalctl -u aurad -f -o cat
```

## UPD 🕊 on v0.7.0 (Update Height: 7097820)

```shell
cd $HOME/aura
git pull
git checkout euphoria_v0.7.0
make build
$HOME/aura/build/aurad version --long | grep -e version -e commit
# euphoria_v0.7.0
# commit: 29d9bd2a1218a72dca315d434ba07bc7c0928433

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop aurad
mv $HOME/aura/build/aurad $(which aurad)
aurad version --long | grep -e version -e commit
#

systemctl restart aurad && journalctl -u aurad -f -o cat
```

## UPD 🕊 on v0.7.1-euphoria (Update Height: 7545433)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/aura
</strong>git pull
git checkout v0.7.1-euphoria
make build
$HOME/aura/build/aurad version --long | grep -e version -e commit
# euphoria_v0.7.1
# commit: 4ad31e1ee2ad623a4fb7b5fe27f86f39d26e65e9

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop aurad
mv $HOME/aura/build/aurad $(which aurad)
aurad version --long | grep -e version -e commit
#

systemctl restart aurad &#x26;&#x26; journalctl -u aurad -f -o cat
</code></pre>

## UPD 🕊 on v0.7.2-euphoria (Update Height: **7655365**)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/aura
</strong>git pull
git checkout v0.7.2-euphoria
make build
$HOME/aura/build/aurad version --long | grep -e version -e commit
# euphoria_v0.7.2
# commit: 2ea683f1f536e3db2931ef28d8401d6bc552b12b

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop aurad
mv $HOME/aura/build/aurad $(which aurad)
aurad version --long | grep -e version -e commit
#

systemctl restart aurad &#x26;&#x26; journalctl -u aurad -f -o cat
</code></pre>

## UPD 🕊 on v0.7.3-euphoria (Update Height: **8831492**)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/aura
</strong>git pull
git checkout v0.7.3-euphoria
make build
$HOME/aura/build/aurad version --long | grep -e version -e commit
# euphoria_v0.7.3
# commit: 210c446899494ff34552fd4d0c7041c703c1ccbd

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop aurad
mv $HOME/aura/build/aurad $(which aurad)
aurad version --long | grep -e version -e commit
#

systemctl restart aurad &#x26;&#x26; journalctl -u aurad -f -o cat
</code></pre>

## UPD 🕊 on v0.8.1-euphoria (Update Height: 10519700)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/aura
</strong>git pull
git checkout v0.8.1-euphoria
make build
$HOME/aura/build/aurad version --long | grep -e version -e commit
# 0.8.1-euphoria
# commit: e49a2855b1dd38e7a41c175a62eaf5d1ac45518f

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop aurad
mv $HOME/aura/build/aurad $(which aurad)
aurad version --long | grep -e version -e commit
#

systemctl restart aurad &#x26;&#x26; journalctl -u aurad -f -o cat
</code></pre>

## UPD 🕊 on v0.9.3-euphoria (Update Height: 11679500)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/aura
</strong>wget https://github.com/aura-nw/aura/releases/download/v0.9.3-euphoria/aurad
chmod +x aurad
aurad version --long | grep -e commit -e version
#version: v0.9.3-euphoria
#commit: d3e717e44a2b9fadf3c19ec6a8bc2a98e2bac470

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop aurad
mv aurad $(which aurad)
#mv $HOME/aura/build/aurad $(which aurad)
aurad version --long | grep -e version -e commit
#

systemctl restart aurad &#x26;&#x26; journalctl -u aurad -f -o cat
</code></pre>
