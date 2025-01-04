# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.gitopia

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
<mark style="color:green;">**With full synchronization, start from**</mark>**&#x20;v2.0.0**
{% endhint %}

## UPD 🕊 on v2.1.0 (Update Height: unscheduled)

```shell
cd $HOME/gitopia
git pull
git checkout v2.1.0
make build
$HOME/gitopia/build/gitopiad version --long | grep -e version -e commit -e build
# version: 2.1.0
# commit: 603877bf62c76bee7c36360cdd81a5512ab15f83
# build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop gitopiad
mv $HOME/gitopia/build/gitopiad $(which gitopiad)
gitopiad version --long | grep -e version -e commit
# 

systemctl restart gitopiad && journalctl -u gitopiad -f -o cat
```

## UPD 🕊 on v2.1.1 (Update Height: unscheduled)

```shell
cd $HOME/gitopia
git pull
git checkout v2.1.1
make build
$HOME/gitopia/build/gitopiad version --long | grep -e version -e commit -e build
# version: 2.1.1
# commit: 08692dc0220a49e25be15b986cf3d4bfb990d27f
# build_tags: netgo,ledger


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop gitopiad
mv $HOME/gitopia/build/gitopiad $(which gitopiad)
gitopiad version --long | grep -e version -e commit
# 

systemctl restart gitopiad && journalctl -u gitopiad -f -o cat
```

## UPD 🕊 on v3.0.1 (Update Height: 6072000)

```shell
cd $HOME/gitopia
git pull
git checkout v3.0.1
make build
$HOME/gitopia/build/gitopiad version --long | grep -e version -e commit -e build
# version: 3.0.1
# commit: 18f36eddc43ff45a022ed703f04ab0a337285273
# build_tags: netgo,ledger


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop gitopiad
mv $HOME/gitopia/build/gitopiad $(which gitopiad)
gitopiad version --long | grep -e version -e commit
# 

systemctl restart gitopiad && journalctl -u gitopiad -f -o cat
```

## UPD 🕊 on v3.2.0 (Update Height: 6446000)

```shell
cd
rm -r $HOME/gitopia
git clone https://github.com/gitopia/gitopia && cd gitopia
git checkout v3.2.0
make build
$HOME/gitopia/build/gitopiad version --long | grep -e version -e commit -e build
# version: 3.2.0
# commit: ae42d1c44fe003f9075194bfdb2e1f824c627403
# build_tags: netgo,ledger


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop gitopiad
mv $HOME/gitopia/build/gitopiad $(which gitopiad)
gitopiad version --long | grep -e version -e commit
# 

systemctl restart gitopiad && journalctl -u gitopiad -f -o cat
```

## UPD 🕊 on v3.3.0 (Update Height: 6720000)

```shell
cd
rm -r $HOME/gitopia
git clone https://github.com/gitopia/gitopia && cd gitopia
git checkout v3.3.0
make build
$HOME/gitopia/build/gitopiad version --long | grep -e version -e commit -e build
# version: 3.3.0
# commit: 0eab60ecf8c3e22e9c71d54346f76a35830a5dc5
# build_tags: netgo,ledger


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop gitopiad
mv $HOME/gitopia/build/gitopiad $(which gitopiad)
gitopiad version --long | grep -e version -e commit
# 

systemctl restart gitopiad && journalctl -u gitopiad -f -o cat
```

## UPD 🕊 on v4.0.0 (Update Height: 24330422)

```shell
cd
rm -r $HOME/gitopia
git clone https://github.com/gitopia/gitopia && cd gitopia
git checkout v4.0.0
make build
$HOME/gitopia/build/gitopiad version --long | grep -e version -e commit -e build
# version: v4.0.0
# commit: 20a03bd0025b91e3a48b4046f08f4f6115f35e79
# build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop gitopiad
mv $HOME/gitopia/build/gitopiad $(which gitopiad)
gitopiad version --long | grep -e version -e commit
# 

systemctl restart gitopiad && journalctl -u gitopiad -f -o cat
```

## UPD 🕊 on v5.1.0 (Update Height: 29437704)

```shell
cd
rm -r $HOME/gitopia
git clone https://github.com/gitopia/gitopia && cd gitopia
git checkout v5.1.0
make build
$HOME/gitopia/build/gitopiad version --long | grep -e version -e commit -e build
# version: 5.1.0
# commit: 3b1ea32980e79ea74a1398f78a0388867e8d5fd4
# build_tags: netgo,ledger

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop gitopiad
mv $HOME/gitopia/build/gitopiad $(which gitopiad)
gitopiad version --long | grep -e version -e commit
# 

systemctl restart gitopiad && journalctl -u gitopiad -f -o cat
```
