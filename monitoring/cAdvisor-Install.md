# cAdvisor Install (Docker Container Monitoring)

# Run cAdvisor Container
```bash
docker run -d \
  --name=cadvisor \
  --restart=always \
  -p 8080:8080 \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:ro \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  gcr.io/cadvisor/cadvisor:v0.49.1

http://SERVER_IP:8080
http://localhost:8080
```

### Prometheus এ cAdvisor Add যুক্ত করতে হবে

```bash
sudo vim /etc/prometheus/prometheus.yml
scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "node_exporter"
    static_configs:
      - targets: ["localhost:9100"]

  - job_name: "cadvisor"
    static_configs:
      - targets: ["localhost:8080"]

  # Prometheus Restart
  sudo systemctl restart prometheus
```

### 🔐 Production Tips (Important)

- ✔ cAdvisor public expose করবেন না
- ✔ Prometheus + Grafana internal network এ রাখুন
- ✔ Container labels ব্যবহার করুন
- ✔ Alertmanager যুক্ত করুন
- ✔ Docker-compose ব্যবহার করা ভালো

