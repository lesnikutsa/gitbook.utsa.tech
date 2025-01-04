# ⚒️ Node-exporter + Grafana + Prometheus + Alertmanager

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

In this guide, we will set up a monitoring system that will collect metrics from all our servers and visualize them in Grafana. You can also install Alertmanager to receive notifications about server problems, in our case, in Telegram

We need a separate server for Prometheus, Grafana, Node Exporter, Alertmanager. All other servers will only have Node Exporter installed

***

**Prometheus** is an open source DBMS written in Go. An interesting feature of Prometheus is that it pulls metrics from a given set of services. Due to this, Prometheus cannot have any data queues clogged, which means monitoring will never become a bottleneck in the system

**Node Exporter** is a service whose job is to export machine information in a format that Prometheus can understand. There are actually many other exporters for Prometheus, but Node Exporter is perfect for our purposes of server monitoring

**Grafana** is an open web frontend to various time series DBMSs, such as Graphite, InfluxDB, and, of course, Prometheus. With Grafana, we can see beautiful graphs through our browser. It is characteristic that Prometheus also has its own web interface, but even the Prometheus developers themselves recommend using Grafana

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Default ports:**

* prometheus - **9090** (in our guide changed to 808&#x30;**)**

echo -e "\033\[0;32mhttp://$(wget -qO- eth0.me):8080/\033\[0m"

* node exporter - **9100**

`echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):9100/\033[0m"`

* grafana - **3000**

`echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):3000/\033[0m"`
{% endhint %}



## Node Exporter

Can find the latest binaries along with their checksums on the Prometheus [download page](https://prometheus.io/download/)

```bash
# use the --no-create-home and --shell /bin/false parameters,
# so that this user cannot log into the server
useradd --no-create-home --shell /bin/false node_exporter
```

```bash
cd
wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
tar xvf node_exporter-1.5.0.linux-amd64.tar.gz

# copy the binaries to /usr/local/bin
cp node_exporter-1.5.0.linux-amd64/node_exporter /usr/local/bin
chown node_exporter:node_exporter /usr/local/bin/node_exporter
node_exporter --version
#node_exporter, version 1.5.0 (branch: HEAD, revision: 1b48970ffcf5630534fb00bb0687d73c66d1c959)

# delete unnecessary files
rm -r node_exporter-*
```

**Create a service file**

```bash
tee /etc/systemd/system/node_exporterd.service > /dev/null <<EOF
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=default.target
EOF
```

```bash
systemctl daemon-reload
systemctl enable node_exporterd
systemctl restart node_exporterd && journalctl -u node_exporterd -f -o cat
```

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

**Check that the metrics are being given**

```bash
curl 'localhost:9100/metrics'
```

**Now is the time to check the metrics in the browser**

```bash
echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):9100/\033[0m"
# http://108.108.108.108:9100/
```

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>We see that everything is fine and we get the necessary data, which will later be displayed in Grafana</p></figcaption></figure>



## Prometheus, Grafana, AlertManager

### Step 1 - Preparing the Server

```bash
apt update && apt upgrade -y

apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
apt install python3-pip -y
pip install yq
```

### Step 2 - Create a Prometheus User

For security purposes, let's create a prometheus account

```bash
useradd --no-create-home --shell /bin/false prometheus

# create the necessary directories to store Prometheus files and data
mkdir /etc/prometheus
mkdir /var/lib/prometheus

# set user and group permissions in new directories for user prometheus
chown prometheus:prometheus /etc/prometheus
chown prometheus:prometheus /var/lib/prometheus
```

### Step 3 - Install Prometheus

```bash
cd && \
wget https://github.com/prometheus/prometheus/releases/download/v2.38.0/prometheus-2.38.0.linux-amd64.tar.gz && \
tar xvf prometheus-2.38.0.linux-amd64.tar.gz
```

```bash
cp prometheus-2.38.0.linux-amd64/prometheus /usr/local/bin/
cp prometheus-2.38.0.linux-amd64/promtool /usr/local/bin/

chown prometheus:prometheus /usr/local/bin/prometheus
chown prometheus:prometheus /usr/local/bin/promtool

cp -r prometheus-2.38.0.linux-amd64/consoles /etc/prometheus
cp -r prometheus-2.38.0.linux-amd64/console_libraries /etc/prometheus
cp prometheus-2.38.0.linux-amd64/prometheus.yml /etc/prometheus/

chown -R prometheus:prometheus /etc/prometheus

rm -rf prometheus-2.38.0.linux-amd64.tar.gz prometheus-2.38.0.linux-amd64

prometheus --version
promtool --version
```

### Step 4 - Configure Prometheus

Now we need to configure prometheus.yml and add 1 or more parameters to it depending on the number of servers and nodes used

{% hint style="danger" %}
**Important** - the Prometheus configuration file uses YAML format, which strictly forbids tabs and requires two spaces for indentation. Prometheus will not start if the configuration file is not formatted correctly
{% endhint %}

**Open the default config and bring it to the following form**

```bash
nano /etc/prometheus/prometheus.yml
```

```bash
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:8080','localhost:9100']
```

{% hint style="danger" %}
**Important** - we have replaced the default prometheus port (9090) with port (8080)
{% endhint %}

**Create a service file**

```bash
tee /etc/systemd/system/prometheusd.service > /dev/null <<EOF
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries \
    --web.listen-address=:8080

[Install]
WantedBy=multi-user.target
EOF
```

```bash
systemctl daemon-reload &&
systemctl enable prometheusd &&
systemctl restart prometheusd && systemctl status prometheusd
#journalctl -u prometheusd -f -o cat
```

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

**Check that the metrics are being given**

```bash
curl 'localhost:8080/metrics'
```

**Now is the time to check the information in the browser**

```
echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):8080/\033[0m"
# http://108.108.108.108:9090/
```

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Go to Targets</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

In the future, you can add additional information about your other servers to prometheus.yml to monitor several servers at once. The configuration will depend on your settings and json for Grafana



### Step 5 - Install Grafana

```bash
sudo apt-get install -y adduser libfontconfig1 && \
wget https://dl.grafana.com/enterprise/release/grafana-enterprise_9.1.3_amd64.deb && \
sudo dpkg -i grafana-enterprise_9.1.3_amd64.deb
```

```bash
systemctl daemon-reload &&
systemctl enable grafana-server &&
systemctl restart grafana-server && systemctl status grafana-server
```

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

**Now it's time to go to the browser**

```bash
echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):3000/\033[0m"
# http://108.108.108.108:3000/
```

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p>When you first open it, you must enter your login and password</p></figcaption></figure>

{% hint style="info" %}
Login - admin&#x20;

Password - admin
{% endhint %}

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption><p>The system immediately asks us to come up with a new password. Don't skip this!!!</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>Go to Data sources to configure Grafana</p></figcaption></figure>



<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

**Now we need to specify the URL address to our server with Prometheus in the column. If Prometheus is on the same server as Grafana, then set http://localhost:8080 and save the settings**

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Now the most interesting part and we need to upload json files for our dashboard. To do this, you need to know the ID of the json file or download it from someone or create it yourself**



* You can find json to your liking - [https://grafana.com/grafana/dashboards/](https://grafana.com/grafana/dashboards/)
* [https://grafana.com/grafana/dashboards/1860-node-exporter-full/](https://grafana.com/grafana/dashboards/1860-node-exporter-full/)
* [dashboard от Cyberomanov](https://github.com/cyberomanov/grafana)
{% endhint %}

If we have downloaded json, then we upload it ourselves

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

You can load several json at once and switch between them. Or create your own, spending a certain amount of time on it)



### Step 6 - Install Alert manager

First, let's set up Prometheus rules. To do this, create a **rules.yml** file in the `/etc/prometheus/` section, which will contain our rules for alerts In our configuration file, we will monitor the operation of node exporter, CPU load, RAM, and disk space

```bash
nano /etc/prometheus/rules.yml
```

```bash
groups:
  - name: CriticalAlers
    rules:
      - alert: UPTIME_DOWN
        expr: up == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          description: '🚨𝐒𝐄𝐑𝐕𝐄𝐑 𝐢𝐬 𝐃𝐎𝐖𝐍 {{ $labels.instance }} '
          summary: 'СSERVER IS UNAVAILABLE FOR MORE THAN 1 MINUTE. CHECK node_exporter AND THE SERVER ITSELF'
      - alert: DISK_space_usage_is_High
        expr: 100 * (1 - (node_filesystem_avail_bytes / node_filesystem_size_bytes{mountpoint="/"})) > 85
        for: 1m
        labels:
          severity: critical
        annotations:
          description: '🚨𝐇𝐈𝐆𝐇 𝐃𝐈𝐒𝐊 𝐔𝐒𝐀𝐆𝐄 {{ $labels.instance }} '
          summary: 'DISK IS 85 PERCENT FULL. CHECK DISK'
      - alert: CPU_usage_is_High
        expr: 100 - (avg(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 85
        for: 1m
        labels:
          severity: critical
        annotations:
          description: '🚨𝐇𝐈𝐆𝐇 𝐂𝐏𝐔 𝐔𝐒𝐀𝐆𝐄 {{ $labels.instance }} '
          summary: 'CPU USAGE IS OVER 85 PERCENT. CHECK SERVER'
      - alert: RAM_usage_is_High
        expr: 100 * (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) > 85
        for: 1m
        labels:
          severity: critical
        annotations:
          description: '🚨𝐇𝐈𝐆𝐇 𝐑𝐀𝐌 𝐔𝐒𝐀𝐆𝐄 {{ $labels.instance }} '
          summary: 'RAM USAGE IS OVER 85 PERCENT. CHECK SERVER'
```

**Adding information about our rules to the existing prometheus config**

```bash
nano /etc/prometheus/prometheus.yml
```

Adding lines

```bash
rule_files:
  - 'rules.yml'

alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - localhost:9093
```

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

**Check the created rules. The output should be #SUCCESS**

```bash
promtool check rules /etc/prometheus/rules.yml
```

**After making all the changes, restart Prometheus**

```bash
systemctl restart prometheusd && systemctl status prometheusd
```

**Downloading binary files**

```bash
wget https://github.com/prometheus/alertmanager/releases/download/v0.26.0/alertmanager-0.26.0.linux-amd64.tar.gz
tar -zxf alertmanager-0.26.0.linux-amd64.tar.gz

cp alertmanager-0.26.0.linux-amd64/alertmanager /usr/local/bin/
cp alertmanager-0.26.0.linux-amd64/amtool /usr/local/bin/

cp alertmanager-0.26.0.linux-amd64/alertmanager.yml /etc/prometheus

chown -R prometheus:prometheus /etc/prometheus/alertmanager.yml
chown -R prometheus:prometheus /etc/prometheus/rules.yml

chown -R prometheus:prometheus /usr/local/bin/alertmanager
chown -R prometheus:prometheus /usr/local/bin/amtool
```

```bash
# delete unnecessary data
cd
rm -r alertmanager-*
```

**Set up alertmanager.yml, in which we register the bot token and chat ID from Telegram, to which notifications will be sent**

```bash
nano /etc/prometheus/alertmanager.yml
```

```bash
global:
  resolve_timeout: 10s

route:
  group_by: ['alertname']
  group_wait: 10s
  group_interval: 15s
  repeat_interval: 60m
  receiver: 'telegram_bot'

receivers:
- name: 'telegram_bot'
  telegram_configs:
  - bot_token: 'BOT_TOKEN'
    api_url: 'https://api.telegram.org'
    chat_id: CHAT_ID
    parse_mode: ''
```



{% hint style="info" %}
Now you can try to make a test run and temporarily disable node exporter on another server. You should receive a notification in Telegram

```bash
alertmanager --config.file=/etc/prometheus/alertmanager.yml
```



The web interface will be available on port 9093. Find out the address and paste it into the browser

![](<../.gitbook/assets/image (19).png>)

```
echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):9093/\033[0m"

```
{% endhint %}

**Create a service file**

```bash
tee /etc/systemd/system/alertmanager.service > /dev/null <<EOF
[Unit]
Description=alertmanager
After=network.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/alertmanager --config.file=/etc/prometheus/alertmanager.yml --log.level=debug

[Install]
WantedBy=multi-user.target
EOF
```

```bash
systemctl daemon-reload
systemctl enable alertmanager
systemctl restart alertmanager && systemctl status alertmanager
journalctl -u alertmanager -f -o cat
```



## Setting up UFW

In order to ensure the protection of your data, you must at least close our ports from others, since without this, anyone knowing the server IP and port will be able to receive Node exporter or Prometheus data from Grafana We can configure UFW so that data from Prometheus and Grafana are available only from the IP we need (for example, a home PC). And data from Node exporter is available only to the server where Prometheus is installed

```bash
# Allow receiving data from Grafana only from home IP
ufw allow from <YOUR_IP> to any port 3000

# Allow Prometheus to receive data only from home IP
ufw allow from <YOUR_IP> to any port 8080

# Allow receiving data from Node Exporter only for the server with Prometheus
ufw allow from <IP_PROMETHEUS> to any port 9100
```



## Useful commands

**Temporarily load the CPU**

```
apt install stress
stress --cpu 4 --timeout 600s
```

**Temporarily load RAM**

```
apt install stress-ng
stress-ng --vm-bytes $(awk '/MemFree/{printf "%d\n", $2 * 0.9;}' < /proc/meminfo)k --vm-keep -m 1
```

**Remove Alert manager**

```
systemctl stop alertmanager
systemctl disable alertmanager
rm /etc/systemd/system/alertmanager.service
systemctl daemon-reload
```

**Remove Prometheus**

```
systemctl stop prometheusd
systemctl disable prometheusd
systemctl daemon-reload
rm /etc/systemd/system/prometheusd.service
```

**Remove Grafana**

```
systemctl stop grafana-server
systemctl disable grafana-server
systemctl daemon-reload
```



