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

## UPD 🕊 on v1.1.2-hotfix (Update Height: 118040)

```shell
cd $HOME/canine-chain
git pull
git checkout v1.1.2-hotfix
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# 1.1.2
# commit: df8025d5195bfeeb1c7f14b81d4f1db7fa877bd6


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## OPTIONAL UPDATE 🕊 to v1.1.3.1

```shell
cd $HOME/canine-chain
git pull
git checkout v1.1.3.1
make install
canined version --long | grep -e version -e commit
# v1.1.3.1
# commit: 483d113a6edd811ec2754f445cfe6321285f4bff

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v1.2.1 (Update Height: 2080380)

```shell
cd $HOME/canine-chain
git pull
git checkout v1.2.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# 1.2.1
# commit: dfc2d431f8f9c663b5891a399937639692ddfe87


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## OPTIONAL UPDATE 🕊 to v1.2.2

```shell
cd $HOME/canine-chain
git pull
git checkout v1.2.2
make install
canined version --long | grep -e version -e commit
# 1.2.2
# commit: 448f2ec6c29b651d8f583e78f0d74302a90b327b

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v2.0.0 (Update Height: 2631260)

```shell
cd $HOME/canine-chain
git pull
git checkout v2.0.0
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# 2.0.0
# commit: 2c10d62602ff9c4aa68ec977e1f6870e2441df3b


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v2.0.1 (Update Height: unscheduled)

```shell
cd $HOME/canine-chain
git pull
git checkout v2.0.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# 2.0.1
# commit: a0eeff75eb30e606820f8e73f12549e6ec8fd300


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v2.0.2 (Update Height: unscheduled)

```shell
cd $HOME/canine-chain
git pull
git checkout v2.0.2
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# 2.0.2
# commit: cedd6d9a4ed15caf9216c402e5926d18534d9af4


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v2.1.0 (Update Height: 3503000)

```shell
cd $HOME/canine-chain
git pull
git checkout v2.1.0
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# 2.1.0
# commit: 18b171189b5fba890ca861a08e8774400e346a86


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v3.0.5 (Update Height: 4074200)

```shell
cd $HOME/canine-chain
git pull
git checkout v3.0.5
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# version: 3.0.5
# commit: 1b50f4988ba639bc11d3322bc5d36b3261b38d37


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v3.1.3 (Update Height: 6095000)

```shell
cd $HOME/canine-chain
git pull
git checkout v3.1.3
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# version: 3.1.3
# commit: 4d3ddc3f943c5e78debc1e7c9c84555bc273df63


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v3.2.1 (Update Height: 6835000)

```shell
cd $HOME/canine-chain
git pull
git checkout v3.2.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# version: 3.2.1
# commit: 5e6eb390cca253231feb8c570a0d2d04d4b0cfa9


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v3.4.0 (Update Height: 8439000)

```shell
cd $HOME/canine-chain
git pull
git checkout v3.4.0
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# version: 3.4.0
# commit: 399dd63fb26d39b06820fef595d49589c129b869


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v4.0.3 (Update Height: 8527000)

```shell
cd $HOME/canine-chain
git pull
git checkout v4.0.3
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# version: 4.0.3
# commit: 40d9585d0f8f7f3931a4a46dfc047b671d330356


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v4.1.1 (Update Height: 9712500)

```shell
cd $HOME/canine-chain
git pull
git checkout v4.1.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# version: 4.1.1
# commit: 


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v4.2.0 (Update Height: 9920450)

```shell
cd $HOME/canine-chain
git pull
git checkout v4.2.1
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# version: 4.2.1
# commit: 61000aff7787585bfb771d1d38057a3a2a702272


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```

## UPD 🕊 on v4.3.0 (Update Height: 10644650)

```shell
cd $HOME/canine-chain
git pull
git checkout v4.3.0
make build
$HOME/canine-chain/build/canined version --long | grep -e version -e commit
# version: 4.3.0
# commit: 2572380db56a116b276e18af131752881ed1243d


# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop canined
mv $HOME/canine-chain/build/canined $(which canined)
canined version --long | grep -e version -e commit
# 

systemctl restart canined && journalctl -u canined -f -o cat
```
