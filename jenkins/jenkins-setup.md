# Jenkins Install In Docker

### 🧩 Step 1: Pull Jenkins Image with Run

```bash
  # Pull Only Image
  docker pull jenkins/jenkins:lts

  # Pull Image With Run 
  docker run -d \
    --name jenkins \
    -p 8080:8080 \
    -p 50000:50000 \
    -v jenkins_home:/var/jenkins_home \
    -v /var/run/docker.sock:/var/run/docker.sock \
    jenkins/jenkins:lts

  # Get initial Admin Password
  docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword

  # Offline
  # This Jenkins instance appears to be offline.
  sudo vim /etc/docker/daemon.json
  { "dns": ["8.8.8.8", "8.8.4.4"] }

  # Restart Docker and Jenkins Container
  sudo systemctl restart docker
  docker restart jenkins
```

### 🧩 Github Webhook trigger
