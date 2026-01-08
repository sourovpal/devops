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
  name: pv-demo
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
  name: pvc-demo
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
  name: pod-demo
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
        claimName: pvc-demo
```







