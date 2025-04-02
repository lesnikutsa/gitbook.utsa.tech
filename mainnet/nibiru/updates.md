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
