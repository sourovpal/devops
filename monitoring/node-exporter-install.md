# 🌐 Node Exporter Install

**Node Exporter** হলো Prometheus-এর জন্য একটি **মেট্রিক্স এক্সপোর্টার**, যা সার্ভারের **CPU, মেমোরি, ডিস্ক, নেটওয়ার্ক ইত্যাদি হার্ডওয়্যার ও OS লেভেলের ডেটা** Prometheus-কে পাঠায়।


### 🚨 Download & Install
```bash
  cd /tmp
  wget https://github.com/prometheus/node_exporter/releases/download/v1.8.1/node_exporter-1.8.1.linux-amd64.tar.gz
  tar xvf node_exporter-1.8.1.linux-amd64.tar.gz
  cd node_exporter-1.8.1.linux-amd64
  sudo cp node_exporter /usr/local/bin/
```

### 🚨 User Create
```bash
  sudo useradd --no-create-home --shell /bin/false node_exporter
  sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

### 🚨 Systemd Service
```bash
  sudo vim /etc/systemd/system/node_exporter.service

  [Unit]
  Description=Node Exporter
  After=network.target

  [Service]
  User=node_exporter
  ExecStart=/usr/local/bin/node_exporter
  
  [Install]
  WantedBy=default.target 
```

### 🚨 User Create
```bash
  sudo useradd --no-create-home --shell /bin/false node_exporter
  sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

### 🚨 Start
```bash
  sudo systemctl daemon-reload
  sudo systemctl start node_exporter
  sudo systemctl enable node_exporter

  sudo systemctl status node_exporter
```

###🚨 Prometheus এ Node Exporter Add করতে হবে
```bash
  sudo vim /etc/prometheus/prometheus.yml

  scrape_configs:
    - job_name: "prometheus"
      static_configs:
        - targets: ["localhost:9090"]
  
    - job_name: "node_exporter"
      static_configs:
        - targets: ["localhost:9100"]

  sudo systemctl restart prometheus

  http://SERVER_IP:9100/metrics
  http://SERVER_IP:9090/targets
```






