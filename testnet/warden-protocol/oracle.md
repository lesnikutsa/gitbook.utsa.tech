# 📬 Oracle

**Download the slinky binary file**

```shell
cd $HOME/wardenprotocol
curl -Ls https://github.com/skip-mev/slinky/releases/download/v1.0.5/slinky-1.0.5-linux-amd64.tar.gz > slinky-1.0.5-linux-amd64.tar.gz
tar -xzf slinky-1.0.5-linux-amd64.tar.gz
mv slinky-1.0.5-linux-amd64/slinky $HOME/go/bin/slinky

slinky version
1.0.5
```

**Defining our GRPC port**

```bash
GRPC_PORT=$(grep 'address = ' "$HOME/.warden/config/app.toml" | awk -F: '{print $NF}' | grep '90"$' | tr -d '"')
echo $GRPC_PORT
#
```

**Create a service for slinky**

```bash
tee /etc/systemd/system/warden-slinky.service > /dev/null <<EOF
[Unit]
Description=Slinky for Warden Protocol service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which slinky) --market-map-endpoint="127.0.0.1:$GRPC_PORT"
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```
systemctl daemon-reload
systemctl enable warden-slinky
systemctl restart warden-slinky && journalctl -u warden-slinky -f -o cat
```



