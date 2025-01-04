# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.lava

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
Genesis start 🕊 v0.33.0
{% endhint %}

## UPD 🕊 on v0.34.0  (Update Height: 375000)

```
git pull
git checkout v0.34.0
make build-all
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.34.0
#commit: 5f2d89e5ca338b52024760e6edf7a6a18ddfe52f

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.35.0  (Update Height: 413000)

```
git pull
git checkout v0.35.0
make build-all
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.35.0
#commit: fcc0ae8829f6306ed59c98eff3b9eee0156a15bc

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v1.0.1  (Update Height: 451000)

```
#cd $HOME/lava/build
#wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v1.0.1/lavad-v1.0.1-linux-amd64"
#chmod +x $HOME/lava/build/lavad
git pull
git checkout v1.0.1
make build-all
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 1.0.1
#commit: c1f982961f8df57887b4a4a687c32909727827bb

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v2.2.0  (Update Height: 888500)

```
#cd $HOME/lava/build
#wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v1.0.1/lavad-v1.0.1-linux-amd64"
#chmod +x $HOME/lava/build/lavad
git pull
git checkout v2.2.0
make build-all
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 2.2.0
#commit: 617333fdff16d8d07f9fa541ff10acbc13ec08f0

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v3.1.0  (Update Height: 1308000)

```
cd $HOME/lava
git pull
git checkout v3.1.0
make build-all
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 3.1.0
#commit: 200972c00b75a819a10385e47e64e199633480f0

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v4.1.0  (Update Height: 1663000)

```
cd $HOME/lava
git pull
git checkout v4.1.0
make build-all
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 4.1.0
#commit: c57c9f807ef914e810276d5895fac7aa9b2a82ab

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v4.2.0  (Update Height: 1853800)

```
cd $HOME/lava
git pull
git checkout v4.2.0
make build-all
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 4.2.0
#commit: 18c395ba01dbd650eac3d276991782faa28270ae

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```
