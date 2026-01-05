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


