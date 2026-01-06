# **Deployment File Configration (Manage Pods and Replicaset)**

deployment.yaml

### 🧩 Step 1: Basic Configration

```bash
apiVersion: apps/v1
  kind: Deployment                            # Must use this name
  metadata:
    name: html-website-deployment             # Deployment Name Must be unique
```

### 🧩 Step 2: Replicas & Selector Match Labels Configration

```bash
spec:
  replicas: 3                                 # number of pods initial stage
  selector:
    matchLabels:
      app: html-website 
```


### 🧩 Step 3: Pod Configration

```bash
 template:
    metadata:
      labels:
        app: html-website                   # Must matchLabels.app: (html-website - step 2)
    spec:
      containers:
        - name: html-website                # Container Name
          image: html-website:latest        # Docker Image Name with tag
          imagePullPolicy: Never            # Never, IfNotPresent, Always
          ports:
            - containerPort: 80
```
📌 Always - Pod start হলেই Docker Hub / Registry থেকে image pull করবে\
📌 IfNotPresent - আগে local image আছে কিনা চেক করবে | থাকলে 👉 local ব্যবহার করবে | না থাকলে 👉 registry থেকে pull করবে\
📌 Never - শুধু local Docker image ব্যবহার করবে

### 🧩 Step 4: Manage Pod Resources

```bash
  resources:
    requests:
      cpu: "250m"
      memory: "256Mi"
    limits:
      cpu: "500m"
      memory: "512Mi"
```

### 🧩 Delete Deployment or Pod

```bash
  👉 kubectl delete deployment/html-website-deployment                   # Delete deployment
  👉 kubectl scale deployment html-website --replicas=0
  👉 kubectl delete pod/html-website-deployment-xxxxxxxxx                # Delete Pod
```

### 🧩 Others Commands

```bash
  👉 kubectl apply -f .
  👉 kubectl apply -f <file-name>.yaml

  👉 kubectl get pods                                                    # Show all Pods
  👉 kubectl get deployments                                             # Show all deployments

  👉 kubectl describe deployment/html-website-deployment                 # Show existing deployment configration details
  👉 kubectl describe pod/html-website-deployment-xxxxxxxxx              # Show existing pod configration details

  👉 kubectl rollout history deployment/xxxx-xxxxxxx-xxxxx               # History existing deployment pod
  👉 kubectl rollout restart deployment/xxxx-xxxxxxx-xxxxx               # Restart existing pods
  👉 kubectl rollout status deployment/xxxx-xxxxxxx-xxxxx                # Check Current Status
  👉 kubectl rollout undo deployment/xxxx-xxxxxxx-xxxxx                  # Check Current Status

  👉 kubectl get deployment html-app -o yaml
```

