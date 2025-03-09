# 📬 Updates

{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

{% hint style="success" %}
Genesis starts with version zenrockd-5.3.8
{% endhint %}

## UPD 🕊 on v5.5.0 (Update Height: 400000)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v5.5.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 5.5.0
# commit: 1c5e92e50435c334cf814377254367392a4dfda5

#AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v5.16.10 (Update Height: 1730150)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v5.16.10/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 5.16.10
# commit: 

#AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v5.16.18 (Update Height: 1732400)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v5.16.18/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 5.16.18
# commit: 42b9da8cdbece8d150793b1ef0391d32432c4ae9

#AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v5.16.20 (Update Height: 1740850)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v5.16.20/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 5.16.20
# commit: 07cc3836e3e24cd617f3ff091186e0844b7f8dd9

#AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```
