---
description: 'Officially stated requirements: 2/4/100 ubuntu 22.04'
---

# 💻 Installation



## Server preparation

```shell
apt update && apt upgrade -y
```

<pre class="language-shell"><code class="lang-shell"><strong>apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev libgmp3-dev tar clang bsdmainutils ncdu unzip llvm libudev-dev make protobuf-compiler lz4 -y
</strong></code></pre>

## Installing Ubuntu 22.04

**Download the binary file**

```shell
mkdir -p $HOME/.avail_mainnet && cd $HOME/.avail_mainnet
chmod 755 $HOME/.avail_mainnet

wget https://github.com/availproject/avail/releases/download/v2.3.1.0/x86_64-ubuntu-2204-avail-node.tar.gz
tar -xvf x86_64-ubuntu-2204-avail-node.tar.gz
mv avail-node /usr/bin/avail
rm -rf x86_64-ubuntu-2204-avail-node.tar.gz

avail --version
# avail 2.3.1-f12b293a885
```



{% hint style="warning" %}
**Important** - if you are synchronizing with 0, then after synchronization is complete, stop the node, remove the r**eserved-nodes** and **reserved-only** flags and restart the node
{% endhint %}

#### Create a service file

```
yourname=<name>
```

```shell
tee /etc/systemd/system/avail-mainnet.service > /dev/null << EOF
[Unit]
Description=Avail mainnet
After=network-online.target
StartLimitIntervalSec=0
[Service]
User=$USER
Restart=always
RestartSec=3
LimitNOFILE=65535
ExecStart=/usr/bin/avail \
  --base-path $HOME/.avail_mainnet/data/ \
  --chain mainnet \
  --port 30933 \
  --rpc-port 9993 \
  --prometheus-port 9695 \
  --validator \
  --name '$yourname'
[Install]
WantedBy=multi-user.target
EOF
```

```shell
systemctl daemon-reload
systemctl enable avail-mainnet
systemctl restart avail-mainnet && journalctl -u avail-mainnet -f -o cat
```

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

Now our node has started to synchronize. We can check our node in [telemetry](https://telemetry.avail.tools/#/0xd12003ac837853b062aaccca5ce87ac4838c48447e41db4a3dcfb5bf312350c6)

## Validator setup

<mark style="color:red;">**IMPORTANT - further actions should only be taken if the team has selected you as a validator in the future test network. You should also wait for the future network itself to create a validator**</mark>



After the node has synchronized, we pull out the key from our node by entering the command

```bash
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9993
```

> If you get a similar result, then everything is great {"jsonrpc":"2.0","result":"**0xa0very0long0hex0string**","id":1} - copy the key (in bold) we will need it in the near future



* Go to the website and first create a stash wallet
* For stash we configure **Set on-chain Identity** for identification
* We create a validator. To do this, select **Network - Staking - Accounts - Validator**

Next, insert our key received from the validator node, select the commission percentage

As soon as a place among the validators becomes available, you will appear in the Staking Overview tab, but for now you can find yourself on the Waiting tab
