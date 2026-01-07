# Kubernetes Secret ENV Variable Set

`configmap.yaml`

### 🧩 Step 1: Basic Configration
```php
  apiVersion: v1
  kind: Secret
  metadata:
    name: application-secret
  type: Opaque
  data:
    APP_NAME: TXlTZWNyZXRBcHA=       # base64 encoded "MySecretApp"
    APP_ENV: cHJvZHVjdGlvbg==       # base64 encoded "production"
    DB_PASSWORD: c2VjdXJlUGFzcw==  # base64 encoded "securePass"
```
`deployment.yaml`

### 🧩 Step 2: Basic Use
```php
  containers:
    - name: html-website
      image: html-website:latest
      envFrom:
        - secretRef:
            name: app-secret       # Must match metadata.name in secret.yaml
```

### 🧩 Deployment → custom single/multiple variable use

```php
# custom single
containers:
  - name: html-website
    image: html-website:latest
    env:
      - name: APP_ENV
        valueFrom:
          secretRef:
            name: application-secret
            key: APP_ENV

  # custom multiple
  containers:
  - name: html-website
    image: html-website:latest
    env:
      - name: APP_NAME
        valueFrom:
          secretRef:
            name: application-secret
            key: APP_NAME

      - name: APP_ENV
        valueFrom:
          secretRef:
            name: application-secret
            key: APP_ENV

```
