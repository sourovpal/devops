# Persistent Volume (PV) & Persistent Volume Claim (PVC)

### 🧩 Volume কী কাজ করে?

Volume হলো এমন একটা স্টোরেজ ব্যবস্থা যেখানে কন্টেইনারের ডাটা রাখা হয় কন্টেইনারের বাইরে।\
কেন দরকার?\
আপনি একটি Docker container চালাচ্ছেন কন্টেইনার delete / restart করলে\
কন্টেইনারের ভিতরের ডাটা সাধারণত হারিয়ে যায় Volume ব্যবহার করলে ডাটা হারায় না।\
Example 👇:
```bash
👉 docker run -v mydata:/var/lib/mysql mysql
```
mydata → Docker Volume\
/var/lib/mysql → MySQL-এর ডাটা লোকেশন\
Container নষ্ট হলেও database data নিরাপদ থাকে।

### 🧩 Persistent Storage কী?

Persistent Storage হলো একটা concept — মানে ডাটা যেন long-term থাকে, container / pod মারা গেলেও যেন থাকে।\
Volume হচ্ছে একটি implementation, Persistent Storage হচ্ছে goal / উদ্দেশ্য।

### 🧩 Persistent Volume (PV) কী?

  আসল ফিজিক্যাল স্টোরেজ হতে পারে:
  - Hard Disk
  - NFS
  - Cloud Disk (GCP, AWS, Azure)

### 🧩 Persistent Volume Claim (PVC) কী?
Pod বলে:
- আমার 10GB স্টোরেজ লাগবে
- Pod PVC ব্যবহার করে ডাটা লিখে
- Pod delete হলেও → ডাটা থাকে

### 🧩 Step 1: Persistent Volume Create (PV)

`persistent-volume.yaml`

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-storage
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /mnt/data
```
- storage: 5Gi → 5GB স্টোরেজ
- ReadWriteOnce → এক pod একসাথে ব্যবহার করতে পারব
- Retain → PVC delete হলেও ডাটা থাকবে
- /mnt/data → node-এর actual folder

### 🧩 Step 2: Persistent Volume Create (PV)

`persistent-volume.yaml`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-storage
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```
এই PVC automatically pv-demo এর সাথে bind হবে (size & accessMode match হলে)

### 🧩 Step 3: Pod-এ PVC ব্যবহার

`pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: website-pod
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: my-storage
  volumes:
    - name: my-storage
      persistentVolumeClaim:
        claimName: pvc-storage
```
- PVC → Pod-এর ভিতরে /usr/share/nginx/html এ mount
- Nginx এখানে যেটা লিখবে → সেটা Persistent থাকবে

### 🧩 Step 4: Apply & Others Commands
```yaml
👉 kubectl apply -f persistent-volume.yaml
👉 kubectl apply -f persistent-volume-claim.yaml
👉 kubectl apply -f pod.yaml

👉 kubectl get pv
👉 kubectl get pvc
👉 kubectl get pod

👉 kubectl delete pod website-pod
👉 kubectl apply -f pod.yaml
```

### 🧩 emptyDir কী?
Container গুলোর মধ্যে ডাটা শেয়ার একই Pod এর ভিতরে
`pod.yaml`
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: emptydir-demo
spec:
  containers:
    - name: writer
      image: busybox
      command: ["sh", "-c", "echo Hello > /data/hello.txt; sleep 3600"]
      volumeMounts:
        - mountPath: /data
          name: shared-data

    - name: reader
      image: busybox
      command: ["sh", "-c", "cat /data/hello.txt; sleep 3600"]
      volumeMounts:
        - mountPath: /data
          name: shared-data

  volumes:
    - name: shared-data
      emptyDir: {}
```
#### এখানে কী হচ্ছে?
- writer container file লিখছে
- reader container সেই file পড়ছে
- দুই container একই emptyDir ব্যবহার করছে

#### কখন emptyDir ব্যবহার করবেন?

##### ✅ যখন ব্যবহার করবেন:
- Temporary file
- Cache
- Container গুলোর মধ্যে data share
##### ❌ যখন ব্যবহার করবেন না:
- Database data
- User uploaded files
- Important logs


### 🧩 hostPath কী?
Container গুলোর মধ্যে ডাটা শেয়ার একই Pod এর ভিতরে
`pod.yaml`
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hostpath-demo
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: web-data
  volumes:
    - name: web-data
      hostPath:
        path: /data/nginx
        type: DirectoryOrCreate
```
- Host machine-এর /data/nginx folder
- Pod-এর ভিতরে → /usr/share/nginx/html
- Folder না থাকলে → auto create হবে


