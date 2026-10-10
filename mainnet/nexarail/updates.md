# 📬 Updates

##

{% hint style="success" %}
### After update you can check prevotes/precommits status
{% endhint %}

```shell
FOLDER=.nexarail

# find out RPC port
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

PORT=<enter_your_port>

curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'
```



{% hint style="warning" %}
Updates are available for information. Boot via State sync or Snapshot to avoid installing all updates. In this case, you must use the actual version of the binary file and genesis
{% endhint %}

### UPD 🕊 on v0.1.1-mainnet2-fundsafety (Update Height: 990000)

```shell
cd $HOME/nexarail
mkdir -p $HOME/nexarail/build

wget -O $HOME/nexarail/build/nexaraild "https://github.com/Bookings-cpu/nexarail/releases/download/v0.1.1-mainnet2-fundsafety/nexaraild-linux-amd64"
chmod +x $HOME/nexarail/build/nexaraild
$HOME/nexarail/build/nexaraild version
# version: ABCI: 1.0.0

sha256sum $HOME/nexarail/build/nexaraild
# 068aee2853e452a055de2b4082259ff4ccf42573362310f609f22940399b26ca

# AFTER STOPPING THE NETWORK ON THE REQUIRED BLOCK!!!
systemctl stop nexaraild
mv $HOME/nexarail/build/nexaraild $HOME/go/bin/nexaraild
nexaraild version
#
sha256sum $HOME/go/bin/nexaraild
#

systemctl restart nexaraild && journalctl -u nexaraild -f -o cat
```

##
