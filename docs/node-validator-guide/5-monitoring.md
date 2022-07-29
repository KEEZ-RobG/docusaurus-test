

# Part 5 - Monitoring
:::info ToDo
- Copy Vlad's guide
- Fix price
- Add from other sources 
:::

This guide is meant for people with no or little experience in monitoring. It will show you step by step how to do monitoring on your machine by giving you the instructions to install and configure all the tools needed. It will assume you are using a modern linux distribution with systemd and APT (like Ubuntu 20.04) on a modern x86 CPU (Intel, AMD).

## Why would you want to do monitoring?

Here are some good reasons why you might want to do monitoring on your machine:

1. **Information visibility**: You want to expose and be able to easily see your machine details.
2. **Issue tracking and debugging**: You want to be able to inspect what happened in the past and see clearly how your machine reacted to some event.
3. **Issue prevention**: You want to be able to see potential resources exhaustion ahead of time.

## Overview

We will install 5 tools with this guide:

- [Node Exporter](https://prometheus.io/docs/guides/node-exporter/) - monitors the node machine's performance

- [JSON Exporter](https://github.com/prometheus-community/json_exporter) - scrapes LUKSO price information from CoinGecko.

- [Blackbox Exporter](https://github.com/prometheus/blackbox_exporter) monitors ping time between the node machine and two DNS servers.

- [Prometheus](https://prometheus.io/docs/introduction/overview/) - collects metrics from the node, JSON and Blackbox exporters.

- [Grafana](https://grafana.com/oss/grafana/) - queries Prometheus for metrics, displays the information on "dashboards," and provides alerts when data is abnormal.

Connect to your node machine and proceed to the next step
```
ssh lukso
```
## 4-1: Node Exporter

Node_exporter



> add description

---

### Add a user

```
sudo adduser --system node_exporter --group --no-create-home
```

### Install

Check https://prometheus.io/download/#node_exporter to make sure 1.3.1 is the latest stable. As of this writing, 1.4.0 was still in pre-release.
```
wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
```

Verify the SHA256 checksum
Go to https://prometheus.io/download/#node_exporter and confirm the output of this command matches the checksum listed on the download page.
```
sha256sum node_exporter-1.3.1.linux-amd64.tar.gz
```

Extract the archive
```
tar xzvf node_exporter-1.3.1.linux-amd64.tar.gz
```

Copy the binary to the following location and set ownership
```
sudo cp node_exporter-1.3.1.linux-amd64/node_exporter /usr/local/bin/
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

Clean up
```rm node_exporter-1.3.1.linux-amd64.tar.gz
rm -rf node_exporter-1.3.1.linux-amd64
```

### Configure system service

```shell=
sudo nano /etc/systemd/system/node_exporter.service
```

The content of service configuration file:

```
[Unit]
Description=Node Exporter

[Service]
Type=simple
Restart=always
RestartSec=5
User=node_exporter
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

### Enable service

Release systemd to reflect the changes
```
sudo systemctl daemon-reload
```

Start and check the status of the service
```
sudo systemctl start node_exporter
sudo systemctl status node_exporter
```

The output of the status should say "active (running)" in green.
```
● node_exporter.service - Node Exporter
     Loaded: loaded (/etc/systemd/system/node_exporter.service; disabled; vendo>
     Active: active (running) since Wed 2021-08-04 10:48:25 EDT; 4s ago
   Main PID: 10984 (node_exporter)
      Tasks: 5 (limit: 18440)
     Memory: 2.4M
     CGroup: /system.slice/node_exporter.service
             └─10984 /usr/local/bin/node_exporter

Aug 04 10:48:25 remy-MINIPC-PN50 node_exporter[10984]: level=info ts=2021-08-04>
Aug 04 10:48:25 remy-MINIPC-PN50 node_exporter[10984]: level=info ts=2021-08-04>
Aug 04 10:48:25 remy-MINIPC-PN50 node_exporter[10984]: level=info ts=2021-08-04>
Aug 04 10:48:25 remy-MINIPC-PN50 node_exporter[10984]: level=info ts=2021-08-04>
Aug 04 10:48:25 remy-MINIPC-PN50 node_exporter[10984]: level=info ts=2021-08-04>
Aug 04 10:48:25 remy-MINIPC-PN50 node_exporter[10984]: level=info ts=2021-08-04>
Aug 04 10:48:25 remy-MINIPC-PN50 node_exporter[10984]: level=info ts=2021-08-04>
Aug 04 10:48:25 remy-MINIPC-PN50 node_exporter[10984]: level=info ts=2021-08-04>
Aug 04 10:48:25 remy-MINIPC-PN50 node_exporter[10984]: level=info ts=2021-08-04>
Aug 04 10:48:25 remy-MINIPC-PN50 node_exporter[10984]: level=info ts=2021-08-04>
```
Press `q` to quit

Set Node Exporter to start on boot
```
sudo systemctl enable node_exporter
```

## 4-2: Json Exporter

#### Prerequisites 

Check `go` version if installed:

```shell=
go version
```

If it is less than `1.17.7` please install following:

```shell=
wget https://dl.google.com/go/go1.17.7.linux-amd64.tar.gz
sudo tar -xvf go1.17.7.linux-amd64.tar.gz
rm go1.17.7.linux-amd64.tar.gz
sudo mv go /usr/local/go-1.17.7
sudo ln -sf /usr/local/go-1.17.7/bin/go /usr/bin/go
go version
```

#### Build and Install

User:

```shell=
sudo adduser --system json_exporter --group --no-create-home
```

Install:

```shell=
cd
git clone https://github.com/prometheus-community/json_exporter.git
cd json_exporter
make build
sudo cp json_exporter /usr/local/bin/
sudo chown json_exporter:json_exporter /usr/local/bin/json_exporter
cd
rm -rf json_exporter
```

#### Configure

```shell=
sudo mkdir /etc/json_exporter
sudo chown json_exporter:json_exporter /etc/json_exporter
```

Setup `LYX` token price:

```shell=
sudo nano /etc/json_exporter/json_exporter.yml
```

The content of configuration file:

```
metrics:
- name: lyxusd
  path: "{.lukso-token.usd}"
  help: Lukso (LYX) price in USD
```

Change ownership of configuration file:

```shell=
sudo chown json_exporter:json_exporter /etc/json_exporter/json_exporter.yml
```

#### Configure Service

```shell=
sudo nano /etc/systemd/system/json_exporter.service
```

The content of service configuration file:

```
[Unit]
Description=JSON Exporter

[Service]
Type=simple
Restart=always
RestartSec=5
User=json_exporter
ExecStart=/usr/local/bin/json_exporter --config.file /etc/json_exporter/json_exporter.yml

[Install]
WantedBy=multi-user.target
```

Enable service:

```shell=
sudo systemctl daemon-reload
sudo systemctl start json_exporter
sudo systemctl enable json_exporter
```

## 4-3: Blackbox Exporter

Pings google and cloudflare to track latency. This is optional.

```shell=
sudo adduser --system blackbox_exporter --group --no-create-home
```

Install:

```shell=
cd
wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.18.0/blackbox_exporter-0.18.0.linux-amd64.tar.gz
tar xvzf blackbox_exporter-0.18.0.linux-amd64.tar.gz
sudo cp blackbox_exporter-0.18.0.linux-amd64/blackbox_exporter /usr/local/bin/
sudo chown blackbox_exporter:blackbox_exporter /usr/local/bin/blackbox_exporter
sudo chmod 755 /usr/local/bin/blackbox_exporter
rm blackbox_exporter-0.18.0.linux-amd64.tar.gz
rm -rf blackbox_exporter-0.18.0.linux-amd64
```

Enable ping permissions:

```shell=
sudo setcap cap_net_raw+ep /usr/local/bin/blackbox_exporter
```

#### Configure

```shell=
sudo mkdir /etc/blackbox_exporter
sudo chown blackbox_exporter:blackbox_exporter /etc/blackbox_exporter
```

```shell=
sudo nano /etc/blackbox_exporter/blackbox.yml
```

The content of configuration file:

```
modules:
        icmp:
                prober: icmp
                timeout: 10s
                icmp:
                        preferred_ip_protocol: ipv4
```

Change ownership of configuration file:

```shell=
sudo chown blackbox_exporter:blackbox_exporter /etc/blackbox_exporter/blackbox.yml
```

#### Configure Service

```shell=
sudo nano /etc/systemd/system/blackbox_exporter.service
```

The content of service configuration file:

```
[Unit]
Description=Blackbox Exporter

[Service]
Type=simple
Restart=always
RestartSec=5
User=blackbox_exporter
ExecStart=/usr/local/bin/blackbox_exporter --config.file /etc/blackbox_exporter/blackbox.yml

[Install]
WantedBy=multi-user.target
```

Enable service:

```shell=
sudo systemctl daemon-reload
sudo systemctl start blackbox_exporter
sudo systemctl enable blackbox_exporter
```
## 4-4: Prometheus

```shell=
sudo adduser --system prometheus --group --no-create-home
```

Identify latest version for `linux-amd64` [here](https://prometheus.io/download/), e.g. `2.34.0`. Install prometheus by replacing `{VERSION}` in the following:

```shell=
cd
wget https://github.com/prometheus/prometheus/releases/download/v{VERSION}/prometheus-{VERSION}.linux-amd64.tar.gz
tar xzvf prometheus-{VERSION}.linux-amd64.tar.gz
cd prometheus-{VERSION}.linux-amd64
sudo cp promtool /usr/local/bin/
sudo cp prometheus /usr/local/bin/
sudo chown root:root /usr/local/bin/promtool /usr/local/bin/prometheus
sudo chmod 755 /usr/local/bin/promtool /usr/local/bin/prometheus
cd
rm prometheus-{VERSION}.linux-amd64.tar.gz
rm -rf prometheus-{VERSION}.linux-amd64
```

#### Configure

```shell=
sudo mkdir -p /etc/prometheus/console_libraries /etc/prometheus/consoles /etc/prometheus/files_sd /etc/prometheus/rules /etc/prometheus/rules.d
```

Edit configuration file:

```shell=
sudo nano /etc/prometheus/prometheus.yml
```

The content of configuration file:

```
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['127.0.0.1:9090']
  - job_name: 'beacon node'
    scrape_interval: 5s
    static_configs:
      - targets: ['127.0.0.1:8080']
  - job_name: 'node_exporter'
    scrape_interval: 5s
    static_configs:
      - targets: ['127.0.0.1:9100']
  - job_name: 'validator'
    scrape_interval: 5s
    static_configs:
      - targets: ['127.0.0.1:8081']
  - job_name: 'ping_google'
    metrics_path: /probe
    params:
      module: [icmp]
    static_configs:
      - targets:
        - 8.8.8.8
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9115  # The blackbox exporter's real hostname:port.
  - job_name: 'ping_cloudflare'
    metrics_path: /probe
    params:
      module: [icmp]
    static_configs:
      - targets:
        - 1.1.1.1
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9115  # The blackbox exporter's real hostname:port.
  - job_name: json_exporter
    static_configs:
    - targets:
      - 127.0.0.1:7979
  - job_name: json
    metrics_path: /probe
    static_configs:
    - targets:
      - https://api.coingecko.com/api/v3/simple/price?ids=lukso-token&vs_currencies=usd
    relabel_configs:
    - source_labels: [__address__]
      target_label: __param_target
    - source_labels: [__param_target]
      target_label: instance
    - target_label: __address__
      replacement: 127.0.0.1:7979
```

Prepare data directory for prometheus:

```shell=
sudo chown -R prometheus:prometheus /etc/prometheus
sudo mkdir /var/lib/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
sudo chmod 755 /var/lib/prometheus
```

Open port to access to metrics. This is optional, only for external use:

```shell=
sudo ufw allow 9090/tcp
```

#### Configure Service

```shell=
sudo nano /etc/systemd/system/prometheus.service
```

The content of service configuration file:

```
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
Restart=always
RestartSec=5
ExecStart=/usr/local/bin/prometheus \
	--config.file /etc/prometheus/prometheus.yml \
	--storage.tsdb.path /var/lib/prometheus/ \
	--storage.tsdb.retention.time=31d \
	--web.console.templates=/etc/prometheus/consoles \
	--web.console.libraries=/etc/prometheus/console_libraries
ExecReload=/bin/kill -HUP $MAINPID

[Install]
WantedBy=multi-user.target
```

Enable service:

```shell=
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
```
## 4-5: Grafana

Install:

```shell=
cd
sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt-get update
sudo apt-get install grafana-enterprise
```

#### Configure Service

```shell=
sudo nano /lib/systemd/system/grafana-server.service
```

The content of service configuration file:

```
[Unit]
Description=Grafana instance
Documentation=http://docs.grafana.org
Wants=network-online.target
After=network-online.target
After=postgresql.service mariadb.service mysql.service

[Service]
EnvironmentFile=/etc/default/grafana-server
User=grafana
Group=grafana
Type=simple
Restart=on-failure
WorkingDirectory=/usr/share/grafana
RuntimeDirectory=grafana
RuntimeDirectoryMode=0750
ExecStart=/usr/sbin/grafana-server \
                            --config=${CONF_FILE} \
                            --pidfile=${PID_FILE_DIR}/grafana-server.pid \
                            --packaging=deb \
                            cfg:default.paths.logs=${LOG_DIR} \
                            cfg:default.paths.data=${DATA_DIR} \
                            cfg:default.paths.plugins=${PLUGINS_DIR} \
                            cfg:default.paths.provisioning=${PROVISIONING_CFG_DIR}


LimitNOFILE=10000
TimeoutStopSec=20
CapabilityBoundingSet=
DeviceAllow=
LockPersonality=true
MemoryDenyWriteExecute=false
NoNewPrivileges=true
PrivateDevices=true
PrivateTmp=true
PrivateUsers=true
ProtectClock=true
ProtectControlGroups=true
ProtectHome=true
ProtectHostname=true
ProtectKernelLogs=true
ProtectKernelModules=true
ProtectKernelTunables=true
ProtectProc=invisible
ProtectSystem=full
RemoveIPC=true
RestrictAddressFamilies=AF_INET AF_INET6 AF_UNIX
RestrictNamespaces=true
RestrictRealtime=true
RestrictSUIDSGID=true
SystemCallArchitectures=native
UMask=0027

[Install]
Alias=grafana.service
WantedBy=multi-user.target
```

Enable service:

```shell=
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

Open port to access to metrics. This is optional, only for external use:

```shell=
sudo ufw allow 3000/tcp
```

#### Configure Dashboard

Login to grafana by navigating to webrowser `http://192.168.86.29:3000`. Replace `192.168.86.29` with IP of your node machine. This is same IP used to ssh.

Default credentials are username and password `admin`. Set a new secure (long) password when prompted by grafana.

##### Data Source

1. On the left-hand menu, hover over the gear menu and click on `Data Sources`
2. Then click on the Add Data Source button
3. Hover over the Prometheus card on screen, then click on the Select button
4. Enter http://127.0.0.1:9090/ into the URL field, then click Save & Test

##### Install Dashboard

1. Hover over the plus symbol icon in the left-hand menu, then click on Import
2. Copy and paste [the dashboard](/grafana/dashboard.json) into the `Import via panel json` text box on the screen
3. Then click the Load button
4. Then click the Import button

##### Enable Alerts

1. On the left-hand menu, hover over the alarm menue and click on `Notification channels`
2. Click on `New channel`
3. Select `Type` and [configure](https://grafana.com/docs/grafana/latest/alerting/old-alerting/notifications/)

On lukso dashboard:

1. Scroll down on a dashboard to `Alerts` section
2. Select each alert and click `Edit`
3. In `Alert` tab, select notifications `send to`
4. Save and repeat for each alert
---
https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/monitoring-your-validator-with-grafana-and-prometheus?q=grafana

https://ethereum.org/en/developers/tutorials/monitoring-geth-with-influxdb-and-grafana/

https://docs.prylabs.network/docs/prysm-usage/monitoring/grafana-dashboard


---
Credits:
https://github.com/remyroy/ethstaker/blob/main/monitoring.md
https://github.com/lykhonis/lukso-node-guide#prometheus