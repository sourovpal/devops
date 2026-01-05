# Nginx Setup and Configer

### 🧩 Install Nginx package
```bash
  sudo apt update
  sudo apt install nginx -y
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
  sudo systemctl start nginx
  sudo systemctl enable nginx
  sudo systemctl status nginx

  # Firewall

  sudo ufw allow 'Nginx Full'
  sudo ufw reload
```
