# Docker ও Docker Compose: ৫৭ ইন্টারভিউ প্রশ্ন ও ব্যাখ্যা

### ১. Docker কি?
**উত্তর:** Docker একটি কন্টেইনারাইজেশন প্ল্যাটফর্ম যা অ্যাপ্লিকেশনগুলোকে তাদের dependencies সহ একটি হালকা, বহনযোগ্য কন্টেইনারে প্যাকেজ করে। এটি ভিন্ন ভিন্ন এনভায়রনমেন্টে অ্যাপ্লিকেশন চালানোর সুবিধা দেয়।

### ২. কন্টেইনারাইজেশন কি?
**উত্তর:** কন্টেইনারাইজেশন হল একটি ভার্চুয়ালাইজেশন পদ্ধতি যেখানে একটি অ্যাপ্লিকেশন এবং এর সমস্ত dependencies একটি কন্টেইনারে বন্দী থাকে, যা যেকোনো অপারেটিং সিস্টেমে চালানো যায়।

### ৩. Docker Image কি?
**উত্তর:** Docker Image হল একটি রিড-অনলি টেমপ্লেট যাতে একটি অ্যাপ্লিকেশন চালানোর জন্য প্রয়োজনীয় কোড, লাইব্রেরি, dependencies সবকিছু থাকে।

### ৪. Docker Container কি?
**উত্তর:** Docker Container হল Docker Image এর একটি চলমান instance। এটি একটি হালকা, স্ট্যান্ডঅ্যালোন এক্সিকিউটেবল প্যাকেজ।

### ৫. Dockerfile কি?
**উত্তর:** Dockerfile একটি টেক্সট ফাইল যাতে Docker Image তৈরি করার সব নির্দেশনা থাকে। এটি লেয়ার বাই লেয়ার ইন্সট্রাকশন ধারণ করে।

```dockerfile
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y python3
COPY . /app
WORKDIR /app
CMD ["python3", "app.py"]
```

### ৬. Docker Hub কি?
**উত্তর:** Docker Hub হল Docker ইমেজের একটি ক্লাউড-বেসড রেজিস্ট্রি যেখানে ব্যবহারকারীরা তাদের ইমেজ পাবলিশ এবং শেয়ার করতে পারে।

### ৭. Docker Volume কি?
**উত্তর:** Docker Volume হল ডাটা persist করার পদ্ধতি। কন্টেইনার ডিলিট হলেও Volume এর ডাটা থাকে।

### ৮. Docker Network কি?
**উত্তর:** Docker Network হল কন্টেইনারগুলোর মধ্যে কমিউনিকেশনের ব্যবস্থা। বিভিন্ন ধরনের নেটওয়ার্ক আছে:
- bridge (ডিফল্ট)
- host
- overlay
- macvlan

### ৯. Docker Compose কি?
**উত্তর:** Docker Compose হল একটি টুল যা multi-container Docker অ্যাপ্লিকেশন define এবং run করার জন্য। YAML ফাইল ব্যবহার করে।

### ১০. Docker Swarm vs Kubernetes
**উত্তর:** 
- **Docker Swarm:** Docker এর নেটিভ ক্লাস্টারিং টুল, সহজ সেটআপ
- **Kubernetes:** Google ডেভেলপড, বেশি ফিচারসমৃদ্ধ, কমপ্লেক্স

### ১১. Docker Registry কি?
**উত্তর:** Docker Registry হল Docker ইমেজ স্টোর ও ডিস্ট্রিবিউট করার সিস্টেম। Private বা Public হতে পারে।

### ১২. .dockerignore ফাইল কি?
**উত্তর:** .dockerignore ফাইলটি নির্দেশ করে কোন ফাইলগুলো Docker build এর সময় ignore করতে হবে (যেমন: node_modules, .git)।

### ১৩. Docker Object কি কি?
**উত্তর:** 
- Images
- Containers
- Networks
- Volumes
- Plugins

### ১৪. Union File System কি?
**উত্তর:** Union File System Docker এর একটি কোর কম্পোনেন্ট যা multiple layers কে একটি single unified filesystem এ merge করে।

### ১৫. Docker Daemon কি?
**উত্তর:** Docker Daemon (dockerd) হল একটি ব্যাকগ্রাউন্ড প্রসেস যা Docker API request গুলো হ্যান্ডল করে এবং Docker object গুলো manage করে।

### ১৬. Docker Client কি?
**উত্তর:** Docker Client হল কমান্ড লাইন ইন্টারফেস (CLI) যার মাধ্যমে ইউজার Docker এর সাথে ইন্টারঅ্যাক্ট করে।

### ১৭. Docker Architecture ব্যাখ্যা করুন
**উত্তর:**
```
Client → REST API → Docker Daemon
                 ↓
Registry → Images → Containers
```

### ১৮. Docker Install করার স্টেপস
**উত্তর:**
```bash
# Ubuntu
sudo apt-get update
sudo apt-get install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

### ১৯. Basic Docker Commands
**উত্তর:**
```bash
docker version
docker info
docker pull ubuntu
docker images
docker run -it ubuntu bash
docker ps
docker ps -a
docker stop <container_id>
docker rm <container_id>
docker rmi <image_id>
```

### ২০. Docker Image Build
**উত্তর:**
```bash
docker build -t myapp:1.0 .
docker build -f Dockerfile.dev -t myapp:dev .
```

## **Part 2: Docker Intermediate (৩৫টি প্রশ্ন)**

### ২১. Dockerfile Instructions
**উত্তর:**
- FROM: Base image নির্ধারণ
- RUN: কমান্ড এক্সিকিউট
- COPY: ফাইল কপি
- ADD: ফাইল কপি (URL/টার support)
- CMD: ডিফল্ট কমান্ড
- ENTRYPOINT: প্রাইমারি কমান্ড
- ENV: এনভায়রনমেন্ট ভেরিয়েবল
- ARG: Build-time ভেরিয়েবল
- WORKDIR: ওয়ার্কিং ডিরেক্টরি
- EXPOSE: পোর্ট এক্সপোজ

### ২২. CMD vs ENTRYPOINT
**উত্তর:**
- **CMD:** ডিফল্ট কমান্ড, override করা যায়
- **ENTRYPOINT:** ফিক্সড কমান্ড, append করা যায়

### ২৩. COPY vs ADD
**উত্তর:**
- **COPY:** শুধু local ফাইল কপি করে
- **ADD:** URL, টার ফাইল, remote URL support করে

### ২৪. Multi-stage Build কি?
**উত্তর:** Multi-stage build multiple FROM স্টেটমেন্ট ব্যবহার করে। এটি ফাইনাল ইমেজের সাইজ কমায়।
```dockerfile
# Build stage
FROM node:14 AS builder
WORKDIR /app
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
```

### ২৫. Docker Image Layers
**উত্তর:** প্রতিটি Dockerfile instruction একটি নতুন layer তৈরি করে। Layers cache হয়, তাই rebuild দ্রুত হয়।

### ২৬. Docker Image Optimization
**উত্তর:**
- Multi-stage build ব্যবহার
- .dockerignore ফাইল ব্যবহার
- Layer caching অপটিমাইজ
- Lightweight base image ব্যবহার (alpine)
- Unnecessary packages install না করা

### ২৭. Docker Port Mapping
**উত্তর:**
```bash
docker run -p 8080:80 nginx  # host:container
docker run -p 80:80/tcp -p 80:80/udp nginx
```

### ২৮. Docker Volume Types
**উত্তর:**
1. **Host Volume:** স্পেসিফিক path
   ```bash
   docker run -v /host/path:/container/path nginx
   ```
2. **Anonymous Volume:** Docker manage করে
   ```bash
   docker run -v /container/path nginx
   ```
3. **Named Volume:** ইউজার define করে
   ```bash
   docker volume create myvolume
   docker run -v myvolume:/container/path nginx
   ```

### ২৯. Bind Mount vs Volume
**উত্তর:**
- **Bind Mount:** Host এর স্পেসিফিক path
- **Volume:** Docker manage করে, বেশি flexible

### ৩০. Docker Network Types
**উত্তর:**
1. **Bridge:** ডিফল্ট, isolate network
2. **Host:** Host এর network ব্যবহার
3. **None:** No networking
4. **Overlay:** Multiple Docker daemon connect
5. **Macvlan:** MAC address assign

### ৩১. Docker Logs দেখানো
**উত্তর:**
```bash
docker logs <container_id>
docker logs --tail 50 <container_id>
docker logs -f <container_id>  # follow mode
docker logs --since 1h <container_id>
```

### ৩২. Docker Exec Command
**উত্তর:**
```bash
docker exec -it <container_id> bash
docker exec <container_id> ls -la
docker exec -u root <container_id> bash
```

### ৩৩. Docker Inspect
**উত্তর:**
```bash
docker inspect <container_id>
docker inspect --format='{{.NetworkSettings.IPAddress}}' <container_id>
docker inspect <image_id>
```

### ৩৪. Docker Stats
**উত্তর:**
```bash
docker stats
docker stats --all
docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"
```

### ৩৫. Docker System Commands
**উত্তর:**
```bash
docker system df  # disk usage
docker system prune  # unused data remove
docker system prune -a  # সব unused
docker system events  # real-time events
```

## **Part 3: Docker Compose (২০টি প্রশ্ন)**

### ৩৬. docker-compose.yml Structure
**উত্তর:**
```yaml
version: '3.8'
services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: secret
```

### ৩৭. Docker Compose Basic Commands
**উত্তর:**
```bash
docker-compose up
docker-compose up -d  # detach mode
docker-compose down
docker-compose ps
docker-compose logs
docker-compose logs -f web  # specific service
```

### ৩৮. Docker Compose Build
**উত্তর:**
```bash
docker-compose build
docker-compose build --no-cache
docker-compose up --build
```

### ৩৯. Environment Variables in Compose
**উত্তর:**
```yaml
services:
  app:
    environment:
      - DATABASE_URL=mysql://db:3306/mydb
      - DEBUG=true
    env_file:
      - .env
      - .env.production
```

### ৪০. Docker Compose Networks
**উত্তর:**
```yaml
version: '3.8'
networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge

services:
  web:
    networks:
      - frontend
  api:
    networks:
      - frontend
      - backend
  db:
    networks:
      - backend
```

### ৪১. Docker Compose Volumes
**উত্তর:**
```yaml
version: '3.8'
volumes:
  db-data:
    driver: local

services:
  db:
    image: mysql
    volumes:
      - db-data:/var/lib/mysql
      - ./backup:/backup
```

### ৪২. Docker Compose Depends_on
**উত্তর:**
```yaml
services:
  web:
    depends_on:
      - db
      - redis
  db:
    image: postgres
  redis:
    image: redis
```

### ৪৩. Docker Compose Healthcheck
**উত্তর:**
```yaml
services:
  web:
    image: nginx
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 30s
      timeout: 10s
      retries: 3
```

### ৪৪. Docker Compose Profiles
**উত্তর:**
```yaml
services:
  web:
    profiles: ["development"]
    image: nginx:alpine
  
  db:
    profiles: ["development", "production"]
    image: mysql:8.0
```

### ৪৫. Docker Compose Scale
**উত্তর:**
```bash
docker-compose up --scale web=3 --scale worker=2
```

## **Part 4: Advanced Docker (১৫টি প্রশ্ন)**

### ৪৬. Docker Security Best Practices
**উত্তর:**
- Non-root user ব্যবহার
- Official images ব্যবহার
- Regular vulnerability scan
- Secrets management
- Read-only filesystem
- Resource limits
- Image signing

### ৪৭. Docker Secrets Management
**উত্তর:**
```bash
# Create secret
echo "mysecretpassword" | docker secret create db_password -

# In compose file
version: '3.8'
services:
  db:
    image: mysql
    secrets:
      - db_password
secrets:
  db_password:
    external: true
```

### ৪৮. Docker Resource Limits
**উত্তর:**
```yaml
services:
  web:
    deploy:
      resources:
        limits:
          cpus: '0.50'
          memory: 512M
        reservations:
          cpus: '0.25'
          memory: 256M
```

### ৪৯. Docker Health Check
**উত্তর:**
```dockerfile
FROM nginx
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1
```

### ৫০. Docker Logging Drivers
**উত্তর:**
```bash
docker run --log-driver=json-file --log-opt max-size=10m nginx
docker run --log-driver=syslog nginx
docker run --log-driver=journald nginx
```

### ৫১. Docker Swarm Basic
**উত্তর:**
```bash
# Initialize swarm
docker swarm init

# Join worker node
docker swarm join --token <token> <manager-ip>:2377

# Create service
docker service create --replicas 3 -p 80:80 --name web nginx

# Scale service
docker service scale web=5

# Update service
docker service update --image nginx:latest web
```

### ৫২. Docker Stack Deploy
**উত্তর:**
```bash
# Deploy stack from compose file
docker stack deploy -c docker-compose.yml myapp

# List stacks
docker stack ls

# List services in stack
docker stack services myapp

# Remove stack
docker stack rm myapp
```

## **Part 5: Troubleshooting & Best Practices**

### ৫৩. Common Docker Errors & Solutions
**উত্তর:**
1. **Port already in use:** `docker run -p <different_port>:80`
2. **Out of memory:** Resource limit set করুন
3. **Image not found:** Docker Hub check করুন
4. **Permission denied:** `sudo` বা user group add করুন
5. **Container exits immediately:** Logs check করুন

### ৫৪. Docker Cleanup Commands
**উত্তর:**
```bash
# Clean containers
docker container prune

# Clean images
docker image prune

# Clean volumes
docker volume prune

# Clean networks
docker network prune

# Clean everything
docker system prune -a
```

### ৫৫. Docker Backup & Restore
**উত্তর:**
```bash
# Backup container
docker commit <container_id> backup-image
docker save -o backup.tar backup-image

# Backup volume
docker run --rm -v <volume_name>:/data -v $(pwd):/backup alpine \
  tar czf /backup/backup.tar.gz /data

# Restore
docker load -i backup.tar
```

### ৫৬. Docker Monitoring Tools
**উত্তর:**
- Docker Stats
- cAdvisor
- Prometheus
- Grafana
- Portainer (GUI management)

### ৫৭. CI/CD with Docker
**উত্তর:**
```yaml
# GitHub Actions Example
name: Build and Push
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Build Docker image
      run: docker build -t myapp:${{ github.sha }} .
    - name: Push to Registry
      run: docker push myapp:${{ github.sha }}
```

## **Important Interview Tips:**

1. **Hands-on Experience:** প্র্যাকটিক্যাল নলেজ খুব গুরুত্বপূর্ণ
2. **Real Project Examples:** নিজের প্রজেক্টের উদাহরণ দিন
3. **Docker Architecture:** Architecture ভালো করে বুঝুন
4. **Networking & Storage:** এই দুইটা টপিক খুব ask করা হয়
5. **Security:** Security best practices জানা জরুরি
6. **Orchestration:** Docker Swarm/Kubernetes basics জানুন
7. **Problem Solving:** Troubleshooting skill দেখান

