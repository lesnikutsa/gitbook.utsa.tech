# 📬 Updates



**UPD 🕊** v1.6.0-rc.1

**Height 🕊 445000**

```bash
systemctl stop gnoland

cd $HOME/gno
git fetch origin
git checkout v1.6.0-rc.1

make -C gno.land install.gnoland install.gnokey

gnoland version
# gnoland version: v1.6.0-rc.1

systemctl restart gnoland && journalctl -u gnoland -f -o cat
```

**UPD 🕊** v1.7.0-rc.1

**Height 🕊 754000**

```bash
systemctl stop gnoland

cd $HOME/gno
git fetch origin
git checkout v1.7.0-rc.1

make -C gno.land install.gnoland install.gnokey

gnoland version
# gnoland version: v1.7.0-rc.1

systemctl restart gnoland && journalctl -u gnoland -f -o cat
```

**UPD 🕊** v1.7.1-rc.1

**Height 🕊**&#x20;

```bash
systemctl stop gnoland

cd $HOME/gno
git fetch --all --tags
git checkout v1.7.1-rc.1
git rev-parse HEAD
#83431a7f0261882541a8a8dcf1643e6f246bbbe5

VERSION=v1.7.1-rc.1 make -C gno.land install.gnoland install.gnokey

gnoland version
# gnoland version: v1.7.1-rc.1
gnokey version

systemctl restart gnoland && journalctl -u gnoland -f -o cat
```

**UPD 🕊** v1.8.0-rc.1

**Height 🕊** 1203400

```bash
systemctl stop gnoland

cd $HOME/gno
git fetch --all --tags
git checkout v1.8.0-rc.1
git rev-parse HEAD
#fc79041f4cd1a7b3ed4b316dc5ac14162eb78730

VERSION=v1.8.0-rc.1 make -C gno.land install.gnoland install.gnokey

gnoland version
# gnoland version: v1.8.0-rc.1
gnokey version

systemctl restart gnoland && journalctl -u gnoland -f -o cat
```
