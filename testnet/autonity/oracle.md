# 📬 Oracle



```shell
cd
git clone https://github.com/autonity/autonity-oracle && cd autonity-oracle
git fetch --all
git checkout v0.2.4
make autoracle-bakerloo
mv build/bin/autoracle /usr/local/bin

autoracle version
#v0.2.4
```



Let's edit the config, in which we will write the necessary directories, the password from the wallet and, most importantly, the plugins

```bash
nano $HOME/autonity-oracle/build/bin/oracle_config.yml
```

{% hint style="warning" %}
In oracle\_config.yml you need to uncomment the lower lines, register using the links, get the API keys and add them below

**Webs:**

[https://currencyfreaks.com](https://currencyfreaks.comhttps/openexchangerates.orghttps://currencylayer.comhttps://www.exchangerate-api.com)

[https://openexchangerates.org](https://currencyfreaks.comhttps/openexchangerates.orghttps://currencylayer.comhttps://www.exchangerate-api.com)

[https://currencylayer.com](https://currencyfreaks.comhttps/openexchangerates.orghttps://currencylayer.comhttps://www.exchangerate-api.com)

[https://www.exchangerate-api.com](https://currencyfreaks.comhttps/openexchangerates.orghttps://currencylayer.comhttps://www.exchangerate-api.com)

[https://financeapi.net/dashboard](https://financeapi.net/dashboard)
{% endhint %}

Create a service file

```shell
tee <<EOF >/dev/null /etc/systemd/system/antoracle.service
[Unit]  
Description=Autonity Oracle Server  
After=syslog.target network.target  
[Service]  
Type=simple  
ExecStart=/usr/local/bin/autoracle /root/autonity-oracle/build/bin/oracle_config.yml
Restart=on-failure  
RestartSec=5  
[Install]  
WantedBy=multi-user.target
EOF
```

```bash
systemctl daemon-reload
systemctl enable antoracle
systemctl restart antoracle && journalctl -u antoracle -f -o cat
```

