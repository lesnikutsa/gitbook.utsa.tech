# 📬 Updates

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.uptickd

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
<mark style="color:green;">**With full synchronization, start from**</mark>**&#x20;v0.2.4**
{% endhint %}

## UPD 🕊 on v0.2.8 Update Height: 1190080

```shell
cd $HOME/uptick
wget https://github.com/UptickNetwork/uptick/releases/download/v0.2.8/uptick-linux-amd64-v0.2.8.tar.gz
tar -zxvf uptick-linux-amd64-v0.2.8.tar.gz
chmod +x ./uptickd
rm uptick-linux-amd64-v0.2.8.tar.gz
$HOME/uptick/uptickd version --long | grep -e version -e commit -e build_tags
# version: v0.2.8
# build_tags: netgo,ledger
# commit: b7ef90210c63737035f8072e084ca5731e0eb14c

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop uptickd
mv $HOME/uptick/uptickd $HOME/go/bin/

systemctl restart uptickd && journalctl -u uptickd -f -o cat
```

## UPD 🕊 on v0.2.11 Update Height: 2411600

```shell
cd $HOME/uptick
wget https://github.com/UptickNetwork/uptick/releases/download/v0.2.11/uptickd-linux-amd64-v0.2.11.tar.gz
tar -zxvf uptickd-linux-amd64-v0.2.11.tar.gz
chmod +x ./uptickd
rm uptickd-linux-amd64-v0.2.11.tar.gz
$HOME/uptick/uptickd version --long | grep -e version -e commit -e build_tags
# version: v0.2.11
# build_tags: netgo,ledger
# commit: a67f8c973f0b4068493b5063a3f99fa8816558cf

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop uptickd
mv $HOME/uptick/uptickd $HOME/go/bin/

systemctl restart uptickd && journalctl -u uptickd -f -o cat
```

## UPD 🕊 on v0.2.17 Update Height: 4605201

```shell
cd $HOME
rm -rf uptick
git clone https://github.com/UptickNetwork/uptick && cd uptick
git checkout v0.2.17
make build -B
$HOME/uptick/build/uptickd version --long | grep -e version -e commit -e build_tags
# version: v0.2.17
# build_tags: netgo,ledger
# commit: b4b58f20eb48234ba3996456158f380419da11cf

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop uptickd
mv $HOME/uptick/uptickd $HOME/go/bin/

systemctl restart uptickd && journalctl -u uptickd -f -o cat
```

## UPD 🕊 on v0.2.18 Update Height: 4722001

```shell
cd $HOME
rm -rf uptick
git clone https://github.com/UptickNetwork/uptick && cd uptick
git checkout v0.2.18
make build -B
$HOME/uptick/build/uptickd version --long | grep -e version -e commit -e build_tags
# version: v0.2.18
# build_tags: netgo,ledger
# commit: d8618ad5c4054b28c3233f3e4eca40c739672255

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop uptickd
mv $HOME/uptick/uptickd $HOME/go/bin/

systemctl restart uptickd && journalctl -u uptickd -f -o cat
```

## UPD 🕊 on v0.2.19 Update Height: 5232001

```shell
cd $HOME
rm -rf uptick
git clone https://github.com/UptickNetwork/uptick && cd uptick
git checkout v0.2.19
make build -B
$HOME/uptick/build/uptickd version --long | grep -e version -e commit -e build_tags
# version: v0.2.19
# build_tags: netgo,ledger
# commit: 972d58dbe335791b28b6971f1548b4a5e08e9ef5

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop uptickd
mv $HOME/uptick/uptickd $HOME/go/bin/

systemctl restart uptickd && journalctl -u uptickd -f -o cat
```

## UPD 🕊 on v0.3.0 Update Height: 13280391

```shell
cd $HOME
rm -rf uptick
git clone https://github.com/UptickNetwork/uptick && cd uptick
git checkout v0.3.0
make build -B
$HOME/uptick/build/uptickd version --long | grep -e version -e commit -e build_tags
# version: v0.3.0
# commit: c8fec8051a71068bdefa6d6663aace374558eafe

# AFTER THE NETWORK IS STOPPED ON THE REQUIRED BLOCK!!!
systemctl stop uptickd
mv $HOME/uptick/uptickd $HOME/go/bin/

systemctl restart uptickd && journalctl -u uptickd -f -o cat
```
