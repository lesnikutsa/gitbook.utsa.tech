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

## UPD 🕊 on v6.1.16 (Update Height: 2532100)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.1.16/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.1.16
# commit: 6c7047029339dc5798319b7bb705058067d89da1

#AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.4.0 (Update Height: 2856700)

```shell
cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.4.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.4.0
# commit: c35da0babe55718a33756d3778421d8a1da6b33d

#AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.4.2 (Update Height: 2885700)

```shell

cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.4.2/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.4.2
# commit: 5efbcec88dd04e5f4bff99054ebdb6ad61d936aa

#AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.8.1 (Update Height: 3237900)

```shell

cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.8.1/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.8.1
# commit: b82be2c789d001bfa0da92d53f593aff32bcf959

#AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.13.0 (Update Height: 3369100)

```shell

cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.13.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.13.0
# commit: 1939c39f3508bac5ffd4564b9c3d7be8ab8d3027

#AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```

## UPD 🕊 on v6.25.0 (Update Height: 4013800)

```shell

cd $HOME/zenrock
wget https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.25.0/zenrockd
chmod +x zenrockd
$HOME/zenrock/zenrockd version --long | grep -e version -e commit
# version: 6.25.0
# commit: a57887ad030eb1d27666d20c70d12c2643b034d4

#AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
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
# commit: a57887ad030eb1d27666d20c70d12c2643b034d4

#AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop zenrockd
mv $HOME/zenrock/zenrockd $(which zenrockd)
zenrockd version --long | grep -e version -e commit
#
#

systemctl restart zenrockd && journalctl -u zenrockd -f -o cat
```
