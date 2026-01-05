# Nginx Setup and Configer

### 🧩 Install Nginx package
```bash
  👉 sudo apt update
  👉 sudo apt install nginx -y
```
#### 📌 Folder Structure

| Description            | Path                          |
|------------------------|-------------------------------|
| Web root               | /var/www/html                 |
| Main Config            | /etc/nginx/nginx.conf         |
| Site config (Ubuntu)   | /etc/nginx/sites-available/   |
| Enabled sites          | /etc/nginx/sites-enabled/     |
| Logs                   | /var/log/nginx/               |
| Default web root       | /usr/share/nginx/html or /var/www/html |


### 🧩 Nginx Start & Enable
```bash
  👉 sudo systemctl start nginx
  👉 sudo systemctl enable nginx
  👉 sudo systemctl status nginx

  # Firewall

  👉 sudo ufw allow 'Nginx Full'
  👉 sudo ufw reload
```
| Variable                     | Meaning / Use           | Example                      |
| ---------------------------- | ----------------------- | ---------------------------- |
| `$remote_addr`               | Client IP address       | `allow $remote_addr;`        |
| `$remote_port`               | Client port             | Logging / access control     |
| `$request`                   | Full HTTP request line  | `"GET /index.html HTTP/1.1"` |
| `$request_method`            | HTTP method             | `GET, POST`                  |
| `$request_uri`               | Full URI with args      | `/api/user?id=5`             |
| `$uri`                       | URI without args        | `/api/user`                  |
| `$args`                      | Query string            | `id=5&name=soruov`           |
| `$document_root`             | Root path of site       | `/var/www/html`              |
| `$request_filename`          | Full path to file       | `/var/www/html/index.html`   |
| `$scheme`                    | HTTP scheme             | `http` or `https`            |
| `$server_name`               | Current server_name     | `mywebsite.local`            |
| `$server_addr`               | IP of server            | `127.0.0.1`                  |
| `$server_port`               | Port of server          | `80` or `443`                |
| `$host`                      | Host header from client | `mywebsite.local`            |
| `$http_user_agent`           | Client user agent       | `Mozilla/5.0 ...`            |
| `$http_referer`              | HTTP referer header     | `https://google.com`         |
| `$http_cookie`               | Client cookies          | `sessionid=abc123`           |
| `$connection`                | Connection number       | Incremental connection id    |
| `$connection_requests`       | Requests per connection | HTTP keep-alive              |
| `$proxy_add_x_forwarded_for` | Forwarded client IP     | For reverse proxy            |
| `$upstream_cache_status`     | Cache status            | `HIT, MISS, EXPIRED`         |
| `$hostnames`                 | Server hostname(s)      | Used in logging              |
| `$bytes_sent`                | Response bytes          | Logging                      |
| `$content_length`            | Request Content-Length  | POST body length             |
| `$content_type`              | Response content type   | `text/html`                  |
| `$gzip_ratio`                | Compression ratio       | Only if gzip enabled         |
| `$limit_rate`                | Limit rate for response | Throttling                   |
| `$pid`                       | Nginx worker PID        | Logging / monitoring         |
| `$request_time`              | Time to process request | Logging performance          |
| `$status`                    | Response status code    | `200, 404, 500`              |
| `$connection_upgrade`        | For WebSocket upgrade   | `upgrade` header check       |
| `$sent_http_content_type`    | Sent content type       | Logging                      |

### 🧩 Basic Server configer
```bash
  server {
      listen 80;
      server_name mywebsite.local;
  
      root /var/www/mywebsite;    // Source Directory
      index index.html;           // Default homepage
  
      location / {
          try_files $uri $uri/ =404;
      }
  }

  # Enable

  👉 sudo ln -s /etc/nginx/sites-available/mywebsite /etc/nginx/sites-enabled/
  👉 sudo nginx -t         # Configer Test
  👉 sudo systemctl reload nginx
```
📌 $uri	exact file খোঁজে (/about.html)\
📌 $uri/	directory খোঁজে (/blog/)\
📌 =404	কিছুই না পেলে 404 error

### 🧩 Add Local Domain
```bash 
  👉 sudo nano /etc/hosts
  👉 127.0.0.1 mywebsite.local     # Add This file
```

### 🧩 Basic SSL Server configer

```bash
  server {
      listen 443 ssl;
      server_name mywebsite.local;
  
      root /var/www/mywebsite;
      index index.html;
  
      ssl_certificate     /etc/ssl/certs/mywebsite.crt;           # Auth Add When Run Command
      ssl_certificate_key /etc/ssl/private/mywebsite.key;         # Auth Add When Run Command
  
      location / {
          try_files $uri $uri/ =404;
      }
  }
```
### 🧩 Encrypt Free SSL (Production)

```bash
  👉 sudo apt update
  👉 sudo apt install certbot python3-certbot-nginx -y
  👉 sudo certbot --nginx -d mywebsite.local
```

### 🧩 Node.js Multiple Instances Setup
```node
  // index.js
  const express = require("express");
  const app = express();
  const port = process.env.PORT || 3000;
  
  app.get("/", (req, res) => {
    res.send(`Hello from Node.js on port ${port}`);
  });
  
  app.listen(port, () => {
    console.log(`Server running on port ${port}`);
  });

  # PORT=3000 node index.js & PORT=3001 node index.js & PORT=3002 node index.js
  # curl http://localhost:3000
  # curl http://localhost:3001
  # curl http://localhost:3002
```

### 🧩 Nginx Load Balancer Configuration
```bash
  upstream node_app {
      server 127.0.0.1:3000;
      server 127.0.0.1:3001;
      server 127.0.0.1:3002;
  }
  
  server {
      listen 80;
      server_name mynodeapp.local;
  
      location / {
          proxy_pass http://node_app;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection 'upgrade';
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
      }
  }
```
### 🧩 Sticky Session / Least Connections
```bash
  # Round-robin (default)
  # Request 1 → 3000
  # Request 2 → 3001
  # Request 3 → 3002
  # Request 4 → 3000 (loop)

  upstream node_app {
      server 127.0.0.1:3000;
      server 127.0.0.1:3001;
      server 127.0.0.1:3002;
  }


  # Least connections
  upstream node_app {
      least_conn;
      server 127.0.0.1:3000;
      server 127.0.0.1:3001;
      server 127.0.0.1:3002;
  }
  
  # IP hash (sticky session)
  upstream node_app {
      ip_hash;
      server 127.0.0.1:3000;
      server 127.0.0.1:3001;
      server 127.0.0.1:3002;
  }
```
📌 Sticky Session (IP Hash) : Client-এর একই IP address সবসময় একই server instance-এ যাবে। Session (login, cart, game state) ধরে রাখতে লাগে।\
📌 Least Connections : Nginx প্রতিবার request দেয় যে server instance সবচেয়ে কম active connection আছে। High traffic হলে automatic load balance হয় → Faster response

### 🧩 Reverse Proxy

```bash
location /api/ {
    proxy_pass http://127.0.0.1:3000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}
```

### 🧩 Basic Static Cache

```bash
# Static files cache
location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|woff|woff2)$ {
    expires 30d;          # Client browser এ 30 দিন cache
    add_header Cache-Control "public";     # Browser, CDN দুইয়ে cache করা যায়
}
```


