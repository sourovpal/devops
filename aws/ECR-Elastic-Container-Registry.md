# 💾 Elastic Container Registry

### 🧩 **AWS CLI configure করুন** (যদি না থাকে):
```bash
aws configure
```

### 🧩 **ECR রেজিস্ট্রি তৈরি করুন** (যদি না থাকে):
```bash
aws ecr create-repository --repository-name myproject --region us-east-1

{
    "repository": {
        "repositoryArn": "arn:aws:ecr:us-east-1:123456789012:repository/myproject",
        "registryId": "123456789012",
        "repositoryName": "myproject",
        "repositoryUri": "123456789012.dkr.ecr.us-east-1.amazonaws.com/myproject",
        "createdAt": "2025-01-30T10:56:53.427000+06:00",
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": false
        },
        "encryptionConfiguration": {
            "encryptionType": "AES256"
        }
    }
}

# Get repositoryUri Example: 123456789012.dkr.ecr.us-east-1.amazonaws.com/myproject
```

### 🧩 **Docker login to ECR**:
```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com
```
### 🧩 **Simple Project**
`index.html`
```html
  <!DOCTYPE html>
  <html>
    <head>
        <title>Docker</title>
    </head>
    <body>
        <h1>Hello from Docker!</h1>
    </body>
  </html>
```
`Dockerfile`

```Dockefile
# Base image
FROM nginx:alpine

# Copy index.html to Nginx default folder
COPY index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
```

### 🧩 **Docker Image Build, Tag & Run**:
```bash
docker build -t myproject:latest .

docker tag myproject:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/myproject:latest

# Run build image

docker run -d -p 8080:80 123456789012.dkr.ecr.us-east-1.amazonaws.com/myproject:latest

# Push in AWS Elastic Container Registry

docker pull 123456789012.dkr.ecr.us-east-1.amazonaws.com/myproject:latest

```
