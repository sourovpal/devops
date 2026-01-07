# **ConfigMap ENV Veriable Set**

`configmap.yaml`

### 🧩 Step 1: Basic Configration
```bash
  apiVersion: v1
  kind: ConfigMap                           # Type
  metadata:
    name: html-website-configmap            # Unique Name
  data:
    APP_NAME: "My Laravel App"
    APP_ENV: "production"
    DB_HOST: "mysql-service"
```
### 🧩 Step 2: Use All Veriable In Pod's or Deployment
```php
spec:
  containers:
    - name: demo-container
      image: nginx
      envFrom:
        - configMapRef:
            name: html-website-configmap           # Must Match metadata.name (Step: 1)
```
### 🧩 ConfigMap change করলে ENV auto update হয়? ❌ না
```bash
  # ENV variables container start এর সময় set হয়
  # ConfigMap update করলে running Pod এর ENV বদলায় না

  # ✔️ Solution:

  👉 kubectl rollout restart deployment <deployment-name>

```
### 🧩 Multiple ConfigMap Use

`application-configmap.yaml`
```php
apiVersion: v1
kind: ConfigMap
metadata:
  name: application-configmap
data:
  APP_NAME: MyApp
  APP_ENV: production
  LOG_LEVEL: info
```
`database-configmap.yaml`
```php
apiVersion: v1
kind: ConfigMap
metadata:
  name: database-configmap
data:
  DB_HOST: mysql-service
  DB_PORT: "3306"
  DB_DATABASE: myapp_db
```
`deployment.yaml`
```php
  containers:
  - name: html-website
    image: html-website:latest
    envFrom:
      - configMapRef:
          name: application-configmap
      - configMapRef:
          name: database-configmap
```
#### 🧩 Apply
```bash
👉 kubectl apply -f application-configmap.yaml
👉 kubectl apply -f database-configmap.yaml
👉 kubectl apply -f deployment.yaml
👉 kubectl exec -it <pod-name> -- sh -c 'echo $APP_ENV'
```

### 🧩 Deployment → custom single or multiple variable use

```php
# custom single
containers:
  - name: app
    image: myapp:1.0
    env:
      - name: APP_ENV
        valueFrom:
          configMapKeyRef:
            name: application-configmap
            key: APP_ENV

  # custom multiple
  containers:
  - name: app
    image: myapp:1.0
    env:
      - name: APP_NAME
        valueFrom:
          configMapKeyRef:
            name: application-configmap
            key: APP_NAME

      - name: APP_ENV
        valueFrom:
          configMapKeyRef:
            name: application-configmap
            key: APP_ENV

```

### 🧩 Pod-এ Volume হিসেবে Mount করা
```php
volumes:
  - name: config-volume
    configMap:
      name: app-config
volumeMounts:
  - name: config-volume
    mountPath: /etc/config
```






