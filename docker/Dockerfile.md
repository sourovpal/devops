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
WORKDIR /var/www/html

# COPY <source> <destination>
COPY . .
```
📌 যদি /var/www/html না থাকে → Docker নিজে তৈরি করে নেয়

### 🧩 Step 3: CMD
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
