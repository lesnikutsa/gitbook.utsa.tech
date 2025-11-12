# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.nibid

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}



## UPD 🕊 on 1.0.1 (Update Height: 2753803)

```shell
mkdir -p nibiru && cd nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v1.0.1/nibid_1.0.1_linux_amd64.tar.gz
tar -zxvf nibid_1.0.1_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_1.0.1_linux_amd64.tar.gz

$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 1.0.1
# commit: 1898bc12d2bbe747c703b1a4e16aee316a1826ee

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 1.0.2 (Update Height: 3539699)

```shell
wget https://github.com/NibiruChain/nibiru/releases/download/v1.0.2/nibid_1.0.2_linux_amd64.tar.gz
tar -zxvf nibid_1.0.2_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_1.0.2_linux_amd64.tar.gz

$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 1.0.2
# commit: 740f161dba1f2c0932e2f9e11596801aca0ca5bd

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 1.0.3 (Update Height: 4088799)

```shell
wget https://github.com/NibiruChain/nibiru/releases/download/v1.0.3/nibid_1.0.3_linux_amd64.tar.gz
tar -zxvf nibid_1.0.3_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_1.0.3_linux_amd64.tar.gz

$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 1.0.3
# commit: 208012ca00eb1286786950d5ff6b87ecaaadddd7

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 1.1.0 (Update Height: 4447094)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v1.1.0/nibid_1.1.0_linux_amd64.tar.gz
tar -zxvf nibid_1.1.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_1.1.0_linux_amd64.tar.gz

$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 1.1.0
# commit: 3d72b4f3eb17fa3a3f6006b19d8e5d868f392223

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 1.2.0 (Update Height: 4804662)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v1.2.0/nibid_1.2.0_linux_amd64.tar.gz
tar -zxvf nibid_1.2.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_1.2.0_linux_amd64.tar.gz

$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 1.2.0
# commit: 1ba22a79e36e7ed77e2e4503e72bfbbe5c609aa8

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 1.3.0 (Update Height: 6281429)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v1.3.0/nibid_1.3.0_linux_amd64.tar.gz
tar -zxvf nibid_1.3.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_1.3.0_linux_amd64.tar.gz

$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 1.3.0
# commit: 906f9c7fd5cc38b5d1b6308810bcc6facd96c647

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 1.4.0 (Update Height: 7457147)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v1.4.0/nibid_1.4.0_linux_amd64.tar.gz
tar -zxvf nibid_1.4.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_1.4.0_linux_amd64.tar.gz

$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 1.4.0
# commit: 9ff225a9859731e9547966dbc7c41f23e00d6b36

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 1.5.0 (Update Height: 8375044)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v1.5.0/nibid_1.5.0_linux_amd64.tar.gz
tar -zxvf nibid_1.5.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_1.5.0_linux_amd64.tar.gz

$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 1.5.0
# commit: a5279d1c3e2a907d21f7840d86c0dd49c747029f

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 2.0.0 (Update Height: 18538950)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v2.0.0-p1/nibid_2.0.0-p1_linux_amd64.tar.gz
tar -zxvf nibid_2.0.0-p1_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_2.0.0-p1_linux_amd64.tar.gz

$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 2.0.0-p1
# commit: fbcca386905d33051c7c5946d7d8338cef964062

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 2.1.0 (Update Height: 19562174)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v2.1.0/nibid_2.1.0_linux_amd64.tar.gz
tar -zxvf nibid_2.1.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_2.1.0_linux_amd64.tar.gz

$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 2.1.0
# commit: 8cae09bf69878939d6fd0361302876b9366d1aa4

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 2.2.0 (Update Height: 20937412)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v2.2.0-p1/nibid_2.2.0-p1_linux_amd64.tar.gz
tar -zxvf nibid_2.2.0-p1_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_2.2.0-p1_linux_amd64.tar.gz

$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 2.2.0-p1
# commit: 02795698b047305b8f557d04b8fcbb2ec1c27749

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 2.3.0 (Update Height: 22301853)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v2.3.0/nibid_2.3.0_linux_amd64.tar.gz
tar -zxvf nibid_2.3.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_2.3.0_linux_amd64.tar.gz

$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 2.3.0
# commit: f9e6fdc2bc79c378e8b7ec1bc6ce234d9cd5df71

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 2.4.0 (Update Height: 24130375)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v2.4.0/nibid_2.4.0_linux_amd64.tar.gz
tar -zxvf nibid_2.4.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_2.4.0_linux_amd64.tar.gz
​
$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 2.4.0
# commit: f8613010799d5f5a65a544e40e538efb0ae86214

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/

systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 2.5.0 (Update Height: 24477075)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v2.5.0/nibid_2.5.0_linux_amd64.tar.gz
tar -zxvf nibid_2.5.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_2.5.0_linux_amd64.tar.gz
​
$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 2.5.0
# commit: ea784db0b6a11c0e77279da754654ba39d504c33
​
# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/
​
systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 2.6.0 (Update Height: 27836608)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v2.6.0/nibid_2.6.0_linux_amd64.tar.gz
tar -zxvf nibid_2.6.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_2.6.0_linux_amd64.tar.gz
​
$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 2.6.0
# commit: 3cf97e3468f8ce922c8760f839f8f336ca0c37f4
​
# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/
​
systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 2.7.0 (Update Height: 29328457)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v2.7.0/nibid_2.7.0_linux_amd64.tar.gz
tar -zxvf nibid_2.7.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_2.7.0_linux_amd64.tar.gz
​
$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 2.7.0
# commit: 68b9b71a8619971f1c3560d9cf5e5fb222d0a58d
​
# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/
​
systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 2.8.0 (Update Height: 31965671)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v2.8.0/nibid_2.8.0_linux_amd64.tar.gz
tar -zxvf nibid_2.8.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_2.8.0_linux_amd64.tar.gz
​
$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 2.8.0
# commit: 5ff2e0851dc241912aa9e5e636d3656720c41384
​
# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/
​
systemctl restart nibid && journalctl -u nibid -f -o cat
```

## UPD 🕊 on 2.9.0 (Update Height: 32162336)

```shell
cd $HOME/nibiru
wget https://github.com/NibiruChain/nibiru/releases/download/v2.9.0/nibid_2.9.0_linux_amd64.tar.gz
tar -zxvf nibid_2.9.0_linux_amd64.tar.gz
chmod +x ./nibid
rm nibid_2.9.0_linux_amd64.tar.gz
​
$HOME/nibiru/nibid version --long | grep -e version -e commit
# version: 2.9.0
# commit: 68bb5ba3d1b4655ed3aa0c71cd7904688147c0c7
​
# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop nibid
mv $HOME/nibiru/nibid $HOME/go/bin/
​
systemctl restart nibid && journalctl -u nibid -f -o cat
```
