# 🛠️ Crunch bot

Crunch bot - automatic reward payments

Crunch is a command line interface (CLI) for easily automating staking reward payouts on Substrate-based chains

**Crunch allows you to:**

* claim staking rewards for one or a list of validators at the end of each epoch or every X hours
* receive notifications about the amount and rate of total staking rewards received by each validator and its nominators
* get statistics for each validator. For example - inclusion rate, claimed reward rate, epoch score trend, activity for the current epoch
* check for any unclaimed epochs for a given validator

You can check out all the features of Crunch on the [official github page](https://github.com/turboflakes/crunch)



## Installing crunch-bot

Create a directory and download the binary file

```bash
mkdir $HOME/.kusama/crunch-bot && cd $HOME/.kusama/crunch-bot
wget https://github.com/turboflakes/crunch/releases/download/v0.18.1/crunch
chmod +x $HOME/.kusama/crunch-bot/crunch
cp $HOME/.kusama/crunch-bot/crunch /usr/local/bin/
```

```bash
crunch --version
#crunch 0.18.1
```

{% hint style="warning" %}
When running on ubuntu 22.04, an error may occur with the openssl library You can install it yourself

```bash
wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2.23_amd64.deb
dpkg -i libssl1.1_1.1.1f-1ubuntu2.23_amd64.deb
```
{% endhint %}



## Setting .env

Create a main .env configuration file and configure it. The example below provides a simplified configuration file that uses 1 Kusama validator stash wallet. You can see the full functionality [here](https://github.com/turboflakes/crunch/blob/main/.env.example)

{% hint style="info" %}
By default crunch will try to connect to `ws://IP:9944`

If you use RPC to connect, then add the following flag`CRUNCH_SUBSTRATE_WS_URL=ws://IP:9944`
{% endhint %}

{% hint style="info" %}
**Replace:**

* **CRUNCH\_STASHES** on your validator's stash
* **CRUNCH\_MATRIX\_USER** to your main matrix account
* **CRUNCH\_MATRIX\_BOT\_USER** to your additional matrix account, which you will need to create in advance and from which you will receive messages
* **CRUNCH\_MATRIX\_BOT\_PASSWORD** to your password from the additional matrix account
{% endhint %}

```bash
nano $HOME/.kusama/crunch-bot/.env
```

```bash
# ----------------------------------------------------------------
# crunch CLI configuration variables 
# ----------------------------------------------------------------
#
CRUNCH_STASHES=JHRygZAwLR5oScvgF6QcLLR3sFx9GFZWYzirx2cvgF6QcLL
CRUNCH_LIGHT_CLIENT_ENABLED=false
CRUNCH_MAXIMUM_PAYOUTS=4
CRUNCH_MAXIMUM_HISTORY_ERAS=4
CRUNCH_MAXIMUM_CALLS=3
#
# ----------------------------------------------------------------
# Matrix configuration variables
# ----------------------------------------------------------------
#
CRUNCH_MATRIX_DISABLED=false
CRUNCH_MATRIX_PUBLIC_ROOM_DISABLED=true
CRUNCH_MATRIX_USER=@matrix:matrix.org
CRUNCH_MATRIX_BOT_USER=@matrix_bot:matrix.org
CRUNCH_MATRIX_BOT_PASSWORD="password_bot"
#
# ----------------------------------------------------------------
# Nomination Pools configuration variables
# ----------------------------------------------------------------
CRUNCH_POOL_IDS=2
# 1 DOT = 10000000000 PLANCKS
# 1 KSM = 1000000000000 PLANCKS
CRUNCH_POOL_COMPOUND_THRESHOLD=1000000000000
CRUNCH_POOL_ONLY_OPERATOR_COMPOUND_ENABLED=true
```

We also need to create a separate wallet from which to pay for transactions. We top it up and enter the Seed phrase from the wallet in .private.seed

```bash
echo "utsa utsa utsa utsa utsa utsa utsa utsa utsa utsa utsa utsa">> .private.seed
```



## Launching crunch-bot

Now we can see in information form which awards from the last 84 eras were claimed and which were not claimed

```bash
# for Kusama
crunch kusama view 
# for Polkadot
crunch polkadot view
```

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

**Create a service file**

{% hint style="info" %}
'era' - run crunch immediately after EraPaid event is triggered in chain

'daily' - repeat crunch task every 24 hours

'turbo' - repeat crunch task every 6 hours

'once' - tries to payout once and exit
{% endhint %}

```bash
tee /etc/systemd/system/crunch.service > /dev/null <<EOF
[Unit]
Description=Kusama Crunch Bot

[Service]
User=$USER
ExecStart=/usr/local/bin/crunch kusama --config-path $HOME/.kusama/crunch-bot/.env rewards daily --seed-path $HOME/.kusama/crunch-bot/.private.seed
Restart=always
RestartSec=15

[Install]
WantedBy=multi-user.target
EOF
```

```bash
systemctl daemon-reload
systemctl enable crunch
systemctl restart crunch && journalctl -u crunch -f -o cat
```

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

