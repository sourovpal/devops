## 1️⃣ Database Replication (ডাটাবেস রিপ্লিকেশন)

### সংজ্ঞা:
ডাটাবেস রিপ্লিকেশন হলো একটি ডাটাবেসের তথ্যকে অন্য সার্ভারে কপি করা এবং ধরে রাখা, যাতে একাধিক সার্ভারে একই ডাটা থাকে।

### উদ্দেশ্য:

ডাটা Availability বৃদ্ধি করা (একটি সার্ভার ডাউন হলেও অন্য সার্ভার কাজ চালাতে পারে)
Load Balancing (কোনো সার্ভার overloaded হলে, অন্য সার্ভার থেকে data পড়া যায়)
Backup / Disaster Recovery

### ধরন:

#### Master-Slave Replication

একটিমাত্র master থাকে, যেখানে লেখার অপারেশন (INSERT, UPDATE, DELETE) হয়।
এক বা একাধিক slave থাকে, যেখানে শুধু পড়া (SELECT) হয়।

উদাহরণ: MySQL classic replication।

সুবিধা: সহজ এবং দ্রুত পড়ার জন্য scalable।
অসুবিধা: Master fail হলে write operations বন্ধ।

#### Master-Master Replication

একাধিক master থাকে, যেখানেই লেখা ও পড়া উভয় সম্ভব।
উদাহরণ: MySQL, MariaDB multi-master.
সুবিধা: High Availability, লেখা distributed করা যায়।
অসুবিধা: Conflict management জটিল।

#### Asynchronous vs Synchronous Replication

Asynchronous: Master লেখা শেষ করার পর instant slave-এ update হয় না। দ্রুত কিন্তু data loss risk বেশি।
Synchronous: Master লেখার সময় সবার কাছে commit হতে হয়। ধীর কিন্তু zero data loss।
