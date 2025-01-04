# 📬 Updates

{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

{% hint style="success" %}
Genesis starts with version zenrockd-4.7.1
{% endhint %}

## UPD 🕊 on v5.3.4 (Update Height: 1057850)

```shell
cd $HOME/zenrock
wget https://github.com/zenrocklabs/zrchain/releases/download/v5.3.4/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 5.3.4
# commit: 4bcdf8ab99b43e0ff183bb9f3207d6e50ea14ea6

#AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f
```

## UPD 🕊 on v5.4.0 (Update Height: 1210000)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v5.4.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 5.4.0
# commit: 009dc1948f9e8cb772b2a176ac3c1181f48f98e8

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v5.5.0 (Update Height: 1259000)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v5.5.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 5.5.0
# commit: 1c5e92e50435c334cf814377254367392a4dfda5

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```
