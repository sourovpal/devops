# 🌐 Prometheus Install

**Prometheus** হলো একটি **ওপেন-সোর্স মনিটরিং এবং অ্যালার্টিং টুলকিট**, যা মূলত সার্ভার, অ্যাপ্লিকেশন, এবং সার্ভিসের **মেট্রিক (metrics) সংগ্রহ, সংরক্ষণ এবং বিশ্লেষণ** করার জন্য ব্যবহার করা হয়।

### 🚨 Prometheus User Create
```bash
  sudo useradd --no-create-home --shell /bin/false prometheus
```

### 🚨 Directory Create
```bash
  sudo mkdir /etc/prometheus
  sudo mkdir /var/lib/prometheus

  # Add Permission

  sudo chown prometheus:prometheus /etc/prometheus
  sudo chown prometheus:prometheus /var/lib/prometheus
```

### 🚨 Prometheus Download
```bash
  cd /tmp
  wget https://github.com/prometheus/prometheus/releases/download/v2.49.1/prometheus-2.49.1.linux-amd64.tar.gz

  tar xvf prometheus-2.49.1.linux-amd64.tar.gz
  cd prometheus-2.49.1.linux-amd64

  sudo cp prometheus /usr/local/bin/
  sudo cp promtool /usr/local/bin/

  sudo cp -r consoles /etc/prometheus
  sudo cp -r console_libraries /etc/prometheus
  sudo cp prometheus.yml /etc/prometheus/

  sudo chown -R prometheus:prometheus /etc/prometheus
```

### 🚨 Basic Config
`prometheus.yml`
```yml
  vim /etc/prometheus/prometheus.yml

  global:
    scrape_interval: 15s

  scrape_configs:
    - job_name: "prometheus"
      static_configs:
        - targets: ["localhost:9090"]
```

### 🚨 Systemd Service

```service
  sudo vim /etc/systemd/system/prometheus.service

  [Unit]
  Description=Prometheus Monitoring
  Wants=network-online.target
  After=network-online.target
  
  [Service]
  User=prometheus
  Group=prometheus
  Type=simple
  ExecStart=/usr/local/bin/prometheus \
    --config.file=/etc/prometheus/prometheus.yml \
    --storage.tsdb.path=/var/lib/prometheus
  
  [Install]
  WantedBy=multi-user.target
```
### 🚨 Prometheus Start
```sh
  sudo systemctl daemon-reload
  sudo systemctl start prometheus
  sudo systemctl enable prometheus
  sudo systemctl status prometheus

  http://SERVER_IP:9090
  http://localhost:9090
```

