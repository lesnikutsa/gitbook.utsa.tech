# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.empe-chain

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}



## UPD 🕊 on v0.2.2  (Update Height: 1085468)

```shell
cd $HOME/empe-chain
curl -LO https://github.com/empe-io/empe-chain-releases/raw/master/v0.2.2/emped_v0.2.2_linux_amd64.tar.gz
tar -xvf emped_v0.2.2_linux_amd64.tar.gz
rm -r emped_*
chmod 744 $HOME/empe-chain/emped
$HOME/empe-chain/emped version --long | grep -e version -e commit
# version: v0.2.2
# commit: 89a4061fa99a171bb2a15a5c23f0c61749511abd

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop emped
mv $HOME/empe-chain/emped $(which emped)
emped version --long | grep -e version -e commit
# 

systemctl restart emped && journalctl -u emped -f -o cat
```

## UPD 🕊 on v0.3.0  (Update Height: 3871292)

```bash
# LIB
mkdir ~/.empe-chain/lib
wget https://github.com/CosmWasm/wasmvm/releases/download/v1.5.2/libwasmvm.x86_64.so -P $HOME/.empe-chain/lib
echo 'export LD_LIBRARY_PATH=/root/.empe-chain/lib:$LD_LIBRARY_PATH' >> ~/.profile
source ~/.profile

# add to service
nano /etc/systemd/system/emped.service
Environment="LD_LIBRARY_PATH=/root/.empe-chain/lib:$LD_LIBRARY_PATH"
```

```shell
cd $HOME/empe-chain
curl -LO https://github.com/empe-io/empe-chain-releases/raw/master/v0.3.0/emped_v0.3.0_linux_amd64.tar.gz
tar -xvf emped_v0.3.0_linux_amd64.tar.gz
rm -r emped_*
chmod 744 $HOME/empe-chain/emped
$HOME/empe-chain/emped version --long | grep -e version -e commit
# version: v0.3.0
# commit: 8d602e7c6502db75b47369bbbd885cd12256e71a

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop emped
mv $HOME/empe-chain/emped $(which emped)
emped version --long | grep -e version -e commit
# 

systemctl restart emped && journalctl -u emped -f -o cat
```

## UPD 🕊 on v0.4.0  (Update Height: 5215651)

```shell
cd $HOME/empe-chain
curl -LO https://github.com/empe-io/empe-chain-releases/raw/master/v0.4.0/emped_v0.4.0_linux_amd64.tar.gz
tar -xvf emped_v0.4.0_linux_amd64.tar.gz
rm -r emped_*
chmod 744 $HOME/empe-chain/emped
$HOME/empe-chain/emped version --long | grep -e version -e commit
# version: v0.4.0
# commit: ed3616c08b12bc9fe553b33dcf23625b30376f1f

# ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop emped
mv $HOME/empe-chain/emped $(which emped)
emped version --long | grep -e version -e commit
# 

systemctl daemon-reload
systemctl restart emped && journalctl -u emped -f -o cat
```
