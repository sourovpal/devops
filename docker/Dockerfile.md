# Write Dockerfile for Project

### 🧩 Step 1: Base Image declaration
```bash
FROM php:8.2-alpine

# FROM nginx:alpine
# FROM httpd:alpine
# FROM node:18-alpine
# FROM openjdk:17-alpine
# FROM mcr.microsoft.com/dotnet/aspnet:8.0
```
### 🧩 Step 2: Copy Project Local Computer to Docker Image
```bash
# Set working directory
WORKDIR /var/www/html

# COPY <source> <destination>
COPY . .
```
📌 যদি /var/www/html না থাকে → Docker নিজে তৈরি করে নেয়

### 🧩 Step 3: Project RUN CMD
```bash
CMD ["php", "-S", "0.0.0.0:8000"]
```

### 🧩 Step 4: RUN others commands
```bash
RUN apk add --no-cache git unzip zip

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

COPY composer.json composer.lock ./

RUN composer install --no-dev --optimize-autoloader

RUN composer install --no-dev \
 && php artisan key:generate \
 && php artisan config:cache

composer install \
  --no-dev \
  --prefer-dist \
  --optimize-autoloader \
  --no-interaction

```

### 🧩 Step 5: Build Image
```bash
👉 docker build -t <image-name> .
👉 docker build -t <image-name>:<tag-name> .
```

### 🧩 Step 5: Run In Container
```bash
👉 docker run -d --name -p 8080:80 <container-name> <image-name>
👉 docker run -d --name <container-name> -p 8080:80 -v html-website:/var/www/html <image-name>
```

### 🧩 Step 5: All Commands
```bash
👉 docker images                                    # all image list
👉 docker image list                                # all image list
👉 docker ps                                        # all runing container list
👉 docker ps -a                                     # all runing and stoped container list
👉 docker start <container-name or id>              # start container
👉 docker stop <container-name or id>               # stop container
👉 docker rm <container-name or id>                 # delete container
👉 docker rmi <image-name or id>                    # delete image
👉 docker exec -it <container-name> bash/sh         # delete image
```























