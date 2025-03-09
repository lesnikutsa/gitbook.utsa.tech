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
<mark style="color:green;">**With full synchronization, start for testnet 2.0 since v0.21.0**</mark>
{% endhint %}

## UPD 🕊 on v0.22.0  (Update Height: 393139)

```shell
mkdir -p $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.22.0/lavad-v0.22.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.22.0
#commit: bc2e9faeb1850149e0c6dbefb4f3c1b3ff617fc9

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.22.0  (Update Height: 393139)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.23.5/lavad-v0.23.5-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.23.5
#commit: 42b09829da82e46bb10f35af3c19e229ae74412e

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.24.0  (Update Height: 472310)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.24.0/lavad-v0.24.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.24.0
#commit: 61e5b7ba9ee48979d804ad5ba26f3d69ef7b3dbe

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.25.2  (Update Height: 514533)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.25.2/lavad-v0.25.2-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.25.2
#commit: 0081c79f25a56617b10280d0d4e050e6ba71a4c3

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.26.1  (Update Height: 554249)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.26.1/lavad-v0.26.1-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.26.1
#commit: 8ea396ca3670939eb6b3d2fe511a0810a286fc6f

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.27.0  (Update Height: 590764)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.27.0/lavad-v0.27.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.27.0
#commit: 7be36f71d72108553482bb7ab6896db2b61aaf57

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.30.1  (Update Height: 633177)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.30.1/lavad-v0.30.1-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: v0.30.1
#commit: 3ce34fe4b84e44b41f184e4124be22595c8b9197

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.30.2  (Update Height: 636006)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.30.2/lavad-v0.30.2-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: v0.30.2
#commit: 46f0e37beae141e34858cdc6b8aab4879b81d229

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.31.1  (Update Height: 671912)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.31.1/lavad-v0.31.1-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.31.1
#commit: c0aadad208335f564e844eebcb2fe055e0bbe6e5

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.32.0  (Update Height: 711251)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.32.0/lavad-v0.32.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.32.0
#commit: 6d4c5f584f7c2fac8dfc25e38d21cc5dd399e7b6

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.33.0  (Update Height: 764400)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.33.0/lavad-v0.33.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.33.0
#commit: cabb51cc9d73af1fcc16e82463a9350a239f23fb

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.34.0  (Update Height: 809250)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.34.0/lavad-v0.34.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.34.0
#commit: 5f2d89e5ca338b52024760e6edf7a6a18ddfe52f

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v0.35.0  (Update Height: 845700)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v0.35.0/lavad-v0.35.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 0.35.0
#commit: fcc0ae8829f6306ed59c98eff3b9eee0156a15bc

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v1.0.1  (Update Height: 927794)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v1.0.1/lavad-v1.0.1-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 1.0.1
#commit: c1f982961f8df57887b4a4a687c32909727827bb

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v1.2.0  (Update Height: 1048000)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v1.2.0/lavad-v1.2.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: v1.2.0
#commit: 72233db7870f2b2c4344e5dcbc686bef040db109

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v2.0.0  (Update Height: 1191150)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v2.0.0/lavad-v2.0.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 2.0.0
#commit: d4cd0e1b92cb0df0c9f6a69365c14426a054c141

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v2.1.1  (Update Height: 1369750)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v2.1.1/lavad-v2.1.1-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 2.1.1
#commit: 3c937116fc7cd6322cb0b5a005645761cc9e8dff

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v2.1.3  (Update Height: 1407000)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v2.1.3/lavad-v2.1.3-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 2.1.3
#commit: 8e4f5c433616b2d4324371fa0abff36032e260d8

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v2.5.0  (Update Height: 1738800)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v2.5.0/lavad-v2.5.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 2.5.0
#commit: 28a5fdd46d3f71508595753e585df7cb7b170579

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v3.0.1  (Update Height: 1786000)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v3.0.1/lavad-v3.0.1-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 3.0.1
#commit: 21fb8a7df4ea55bf541ee45c91b1e73d158e5bad

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v3.1.0  (Update Height: 1812600)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v3.1.0/lavad-v3.1.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 3.1.0
#commit: 200972c00b75a819a10385e47e64e199633480f0

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v3.2.0  (Update Height: 2037600)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v3.2.0/lavad-v3.2.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 3.2.0
#commit: b3915cbbf58866f3908e9f5243a4bc14d337346d

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v4.0.0  (Update Height: 2070000)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v4.0.0/lavad-v4.0.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 4.0.0
#commit: 1962ab0381ddd80048d400d8d83a5460781cf577

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v4.1.0  (Update Height: 2156000)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v4.1.0/lavad-v4.1.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 4.1.0
#commit: c57c9f807ef914e810276d5895fac7aa9b2a82ab

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```

## UPD 🕊 on v4.2.0  (Update Height: 2311663)

```shell
cd $HOME/lava/build
wget -O $HOME/lava/build/lavad "https://github.com/lavanet/lava/releases/download/v4.2.0/lavad-v4.2.0-linux-amd64"
chmod +x $HOME/lava/build/lavad
$HOME/lava/build/lavad version --long | grep -e version -e commit -e build_tags
#version: 4.2.0
#commit: 18c395ba01dbd650eac3d276991782faa28270ae

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop lavad
mv $HOME/lava/build/lavad $(which lavad)
lavad version --long | grep -e version -e commit
# 

systemctl restart lavad && journalctl -u lavad -f -o cat
```
