# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

## UPD story 🕊 on v0.12.1 (Update Height: 322000)

```shell
cd $HOME/story
git pull
git checkout v0.12.1
go build -o story ./client

$HOME/story/story version
# Version       v0.12.1-stable
# Git Commit    20fed5e

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop story
mv $HOME/story/story $HOME/go/bin/
story version
# 

systemctl restart story && journalctl -u story -f -o cat
```

## UPD story-geth🕊 on v0.11.0-stable

```shell
cd
wget -O story-geth https://github.com/piplabs/story-geth/releases/download/v0.11.0/geth-linux-amd64
chmod +x story-geth
$HOME/story-geth version
# Version: 0.11.0-stable
# Git Commit: f6503708c29cf9b113d710888c51969ca78d79d1
# Git Commit Date: 20241205

mv story-geth $(which story-geth)

systemctl restart story-geth && journalctl -u story-geth -f -o cat
```

## UPD story 🕊 on v0.13.0 (Update Height: 858000)

```shell
cd $HOME/story
git pull
git checkout v0.13.0
go build -o story ./client

$HOME/story/story version
# Version       v0.13.0-stable
# Git Commit    daaa395

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop story
mv $HOME/story/story $(which story)
story version
# 

systemctl restart story && journalctl -u story -f -o cat
```

