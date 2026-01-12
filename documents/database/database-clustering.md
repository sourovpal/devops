## 🧩 Database Clustering (ডাটাবেস ক্লাস্টারিং)

### 👉 সংজ্ঞা:
Clustering হলো একাধিক ডাটাবেস সার্ভারকে একটি logical unit হিসেবে চালানো, যেন তারা একসাথে behave করে।

### 👉 উদ্দেশ্য:

- High Availability (এক সার্ভার fail হলেও cluster কাজ করবে)
- Load Balancing (query requests distribute হবে)
- Scalability (নতুন node যোগ করলে capacity বাড়বে)

### 👉 ধরন:

#### 📌 Active-Passive Cluster

- একটি server active, বাকিগুলো standby।
- Fail হলে passive server take over করে।
- উদাহরণ: PostgreSQL + Patroni, MySQL + MHA

#### 📌 Active-Active Cluster

- সব সার্ভার active এবং concurrentভাবে read/write করতে পারে।
- উদাহরণ: Galera Cluster (MariaDB/MySQL), Oracle RAC
- সুবিধা: High performance, High availability
- অসুবিধা: Conflict handling, network latency

#### 📌 Shared Storage vs Shared Nothing

- Shared Storage: সব node একই storage access করে।
- Shared Nothing: প্রতিটি node আলাদা storage, network দিয়ে sync হয়।
- Shared Nothing বেশি scalable এবং fault-tolerant।
