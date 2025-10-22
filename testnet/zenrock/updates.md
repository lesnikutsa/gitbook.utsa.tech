# 📬 Updates

{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

{% hint style="success" %}
Genesis starts with version zenrockd-5.3.8
{% endhint %}

## UPD 🕊 on v5.5.0 (Update Height: 350)

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

## UPD 🕊 on v5.16.10 (Update Height: 430)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v5.16.10/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 5.16.10
# commit: 353876013de1acca06c5de7cc2a17e735d8517e3

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v5.16.18 (Update Height: 490)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v5.16.18/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 5.16.18
# commit: 42b9da8cdbece8d150793b1ef0391d32432c4ae9

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v5.16.20 (Update Height: 540)

<pre class="language-shell"><code class="lang-shell"><strong>cd $HOME/zenrock
</strong>wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v5.16.20/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 5.16.20
# commit: 07cc3836e3e24cd617f3ff091186e0844b7f8dd9

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd &#x26;&#x26; journalctl -u zenrockd -f -o cat
</code></pre>

## UPD 🕊 on v6.1.16 (Update Height: 590)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.1.16/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.1.16
# commit: 6c7047029339dc5798319b7bb705058067d89da1

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.3.10 (Update Height: 640)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.3.10/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.3.10
# commit: 02647ce3f42ce335382701f9fb1ba9b17bd555c8

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.4.2 (Update Height: )

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.4.2/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.4.2
# commit: 5efbcec88dd04e5f4bff99054ebdb6ad61d936aa

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.5.2 (Update Height: 157000)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.5.2/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.5.2
# commit: c7ea166eb8067aa71be06ffdd92836d525611673

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.5.5 (Update Height: 241000)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.5.5/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.5.5
# commit: 8020e6974145e622437e591826de1dfca5acaf63

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#


systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.8.0 (Update Height: 366500)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.8.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.8.0
# commit: b0cc00b14f00ae3f1881e38bb9ac841000b906a3

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.8.1 (Update Height: 402343)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.8.1/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.8.1
# commit: b82be2c789d001bfa0da92d53f593aff32bcf959

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.13.0 (Update Height: **516500**)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.13.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.13.0
# commit: 1939c39f3508bac5ffd4564b9c3d7be8ab8d3027

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.20.0 (Update Height: 917000)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.20.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.20.0
# commit: 943ea70ea4c5987e0483ed0e9bb5d34d3481b143

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.21.0 (Update Height: 949000)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.21.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.21.0
# commit: afab270875a82c639a1a1d21a163a130edc11615

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.22.0 (Update Height: 1027000)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.22.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.22.0
# commit: d5e09155916a00853449e39d80454bc1b9492b06

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.23.0 (Update Height: 1054000)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.23.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.23.0
# commit: 274967224881573796a4e6e1547621eb31a1a4ab

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.25.0 (Update Height: 1171500)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.25.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.25.0
# commit: a57887ad030eb1d27666d20c70d12c2643b034d4

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.25.4 (Update Height: )

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.25.4/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.25.4
# commit: 9a744e478f6b344d1834b7c96c7443af911a35b2

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.45.0 (Update Height: 2498300)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.45.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.45.0
# commit: 0fd6417eddc1710412e5130d2f9c14b79087b43d

#ПОСЛЕ ОСТАНОВКИ СЕТИ НА НУЖНОМ БЛОКЕ!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```
