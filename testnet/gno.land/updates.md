# 📬 Updates

## soon



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



