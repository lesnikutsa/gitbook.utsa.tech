# ⌚ Snapshots

{% hint style="warning" %}
Be careful with snapshot during network outages. If configured incorrectly, you can get a double signature
{% endhint %}

SNAPSHOT @Mav3rick\_AI - [https://github.com/BeeHiveTeam/genesisl1-snapshots](https://github.com/BeeHiveTeam/genesisl1-snapshots)

~~**SNAPSHOT UTSA**~~

```bash
systemctl stop genesisd
cp $HOME/.genesis/data/priv_validator_state.json $HOME/.genesis/priv_validator_state.json.backup
genesisd unsafe-reset-all --home $HOME/.genesis

rm -rf $HOME/.genesis/data/

cd $HOME
curl -L https://share101.utsa.tech/genesisl1/data-l1.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.genesis --strip-components 2

mv $HOME/.genesis/priv_validator_state.json.backup $HOME/.genesis/data/priv_validator_state.json
systemctl restart genesisd && journalctl -u genesisd -f -o cat
```
