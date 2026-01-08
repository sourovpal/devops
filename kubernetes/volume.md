# Persistent Volume (PV) & Persistent Volume Claim (PVC)

### Volume কী কাজ করে?

Volume হলো এমন একটা স্টোরেজ ব্যবস্থা যেখানে কন্টেইনারের ডাটা রাখা হয় কন্টেইনারের বাইরে।\
কেন দরকার?\
আপনি একটি Docker container চালাচ্ছেন কন্টেইনার delete / restart করলে\
কন্টেইনারের ভিতরের ডাটা সাধারণত হারিয়ে যায় Volume ব্যবহার করলে ডাটা হারায় না।
Example:
```bash
  docker run -v mydata:/var/lib/mysql mysql
```
mydata → Docker Volume\
/var/lib/mysql → MySQL-এর ডাটা লোকেশন\
Container নষ্ট হলেও database data নিরাপদ থাকে।
