# 🔨 Changing ports in the Cosmos ecosystem

Cosmos uses Tendermint. Tendermint is a consensus algorithm that is resistant to malicious activity. By launching several nodes with Tendermint, the user will encounter the intersection of the ports used and, accordingly, the inoperability of the second and subsequent nodes

<mark style="color:blue;">The default ports are: 26656; 26657; 6060; 26658; 26660; 9090; 9091 and others</mark>

{% hint style="danger" %}
**It is not recommended to install a large number of nodes on 1 server - do everything at your own peril and risk!**
{% endhint %}

**config.toml**

* `proxy_app = "tcp://127.0.0.1:26658"`
* `rpc laddr = "tcp://127.0.0.1:26657"`
* `pprof_laddr = "localhost:6060"`
* `p2p laddr = "tcp://0.0.0.0:26656"`
* `prometheus_listen_addr = ":26660"`

**app.toml**

* `api address = "tcp://0.0.0.0:1317"`
* `grpc address = "0.0.0.0:9090"`
* `grpc-web address = "0.0.0.0:9091"`

**client.toml**

* `node = "tcp://localhost:26657"`

Accordingly, for the second and subsequent nodes, it is necessary to change these ports so that they do not intersect. This can be done manually in $HOME/.defund/config/config.toml or using the following commands:

```bash
# set a variable
work_dir=.defund
```

**config.toml**

```bash
# for 2 node
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:36658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:36657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:6061\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:36656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":36660\"%" $HOME/$work_dir/config/config.toml

# for 3 node
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:46658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:46657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:6062\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:46656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":46660\"%" $HOME/$work_dir/config/config.toml

# for 4 node
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:56658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:56657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:6063\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:56656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":56660\"%" $HOME/$work_dir/config/config.toml

# for 5 node
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:60558\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:60557\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:6064\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:60556\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":60560\"%" $HOME/$work_dir/config/config.toml
```

**app.toml**

```bash
# for 2 node
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:9190\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:9191\"%; s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:1327\"%" $HOME/$work_dir/config/app.toml

# for 3 node
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:9290\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:9291\"%; s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:1337\"%" $HOME/$work_dir/config/app.toml

# for 4 node
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:9390\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:9391\"%; s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:1347\"%" $HOME/$work_dir/config/app.toml

# for 5 node
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:9490\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:9491\"%; s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:1357\"%" $HOME/$work_dir/config/app.toml
```

**client.toml**

```bash
# for 2 node
sed -i.bak -e "s%^node = \"tcp://localhost:26657\"%node = \"tcp://localhost:36657\"%" $HOME/$work_dir/config/client.toml

# for 3 node
sed -i.bak -e "s%^node = \"tcp://localhost:26657\"%node = \"tcp://localhost:46657\"%" $HOME/$work_dir/config/client.toml

# for 4 node
sed -i.bak -e "s%^node = \"tcp://localhost:26657\"%node = \"tcp://localhost:56657\"%" $HOME/$work_dir/config/client.toml

# for 5 node
sed -i.bak -e "s%^node = \"tcp://localhost:26657\"%node = \"tcp://localhost:60557\"%" $HOME/$work_dir/config/client.toml
```

**external\_address**

```bash
# for 2 node
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:36656\"/" $HOME/$work_dir/config/config.toml

# for 3 node
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:46656\"/" $HOME/$work_dir/config/config.toml

# for 4 node
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:56656\"/" $HOME/$work_dir/config/config.toml

# for 5 node
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:60556\"/" $HOME/$work_dir/config/config.toml
```

```bash
# don't forget to restart the node after the changes
systemctl restart defundd && journalctl -u defundd -f -o cat
```
