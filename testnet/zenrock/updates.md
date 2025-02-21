# 📬 Updates

{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

{% hint style="success" %}
Genesis starts with version zenrockd-5.3.8
{% endhint %}

## UPD 🕊 on v (Update Height: )

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v5.5.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 
# commit: 

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

