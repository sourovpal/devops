# **ReplicaSet File Configration (Manage Pods)**

replicaset.yaml

### 🧩 Step 1: Basic Configration

```bash
apiVersion: apps/v1
  kind: ReplicaSet                            # Must use this name
  metadata:
    name: html-website-replicaset             # Replicaset Name Must be unique
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

### 🧩 Delete Pod

```bash
  👉 kubectl delete pod/html-website-xxxxxxxxx                # Delete Pod
  👉 kubectl delete replicaset/html-website-xxxxxxxxx         # Delete Replicaset
```

### 🧩 Others Commands

```bash
  👉 kubectl get pods                                                    # Show all Pods
  👉 kubectl get pods                                                    # Show all Pods
```
