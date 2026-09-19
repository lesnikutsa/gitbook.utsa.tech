# 📬 Updates

## soon

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

#### UPD 🕊 00417a1be97b9a311d9669ae7aa9585b277ee594

Height: 113000

```shell
systemctl stop gnoland

cd $HOME/gno
git fetch origin
git checkout 00417a1be97b9a311d9669ae7aa9585b277ee594

make -C gno.land install.gnoland install.gnokey

gnoland version
# gnoland version: HEAD.3436+00417a1be

systemctl restart gnoland && journalctl -u gnoland -f -o cat
```

#### UPD 🕊 e75fef82c02876a4df92ad6e325c5479b9532168

Height: 162200

```shell
systemctl stop gnoland

cd $HOME/gno
git fetch origin
git checkout e75fef82c02876a4df92ad6e325c5479b9532168

make -C gno.land install.gnoland install.gnokey

gnoland version
# gnoland version: HEAD.3444+e75fef82c

systemctl restart gnoland && journalctl -u gnoland -f -o cat
```



