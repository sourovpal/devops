# Linux Commands

### 🧩 File & Folder Command

```bash
    👉 pwd                             # বর্তমান ডিরেক্টরির পথ দেখায়
    👉 ls                              # ডিরেক্টরির ফাইল/ফোল্ডার দেখায়
    👉 ll                              # ডিটেইলড লিস্ট দেখায় (পারমিশন, Owner, সাইজ)
    👉 cd <folder>                     # অন্য ডিরেক্টরিতে চলে যায়
    👉 mkdir <folder>                  # নতুন ফোল্ডার তৈরি করে
    👉 rmdir <folder>                  # ফোল্ডার মুছে ফেলে (খালি হলে)
    👉 rm <file>                       # ফাইল মুছে ফেলে
    👉 rm -r <folder>                  # ফোল্ডার এবং সবকিছু মুছে ফেলে
    👉 cp <source> <destination>       # ফাইল/ফোল্ডার কপি করে
    👉 mv <source> <destination>       # ফাইল/ফোল্ডার নাম পরিবর্তন বা সরানো
    👉 who / whoami                    # Current Login User
```

### 🧩 System Command

```bash
    👉 df -h                           # ডিস্ক স্পেস দেখায়
    👉 du -sh <folder>                 # ফোল্ডারের সাইজ দেখায়
    👉 top                             # লাইভ সিস্টেম প্রসেস দেখায়
    👉 ps aux                          # চলমান প্রসেস দেখায়
    👉 free -h                         # মেমোরি ব্যবহার দেখায়
    👉 uname -a                        # সিস্টেম ইনফো দেখায়
```

### 🧩 File Content Show

```bash
    👉 cat <file>                      # ফাইলের কন্টেন্ট দেখায়
    👉 less <file>                     # বড় ফাইল ধাপে ধাপে দেখার জন্য
    👉 head <file>                     # ফাইলের প্রথম ১০ লাইন দেখায়
    👉 tail <file>                     # ফাইলের শেষ ১০ লাইন দেখায়
    👉 tail -f <file>                  # ফাইল আপডেট লাইভ দেখায়
```

### 🧩 File Search

```bash
    👉 find <path> -name <filename>    # ফাইল খুঁজে বের করা <find . -name "*.txt">
    👉 grep <pattern> <file>           # ফাইলের মধ্যে text search <grep "error" log.txt>
    👉 which <command>                 # কোন path এ command আছে দেখায় <which python>
    👉 man <command>                   # কমান্ডের ম্যানুয়াল দেখায় <man ls>
```

### 🧩 Group

```bash
    👉 sudo groupadd developers            # নতুন group তৈরি
    👉 sudo groupadd -g 1001 designers     # নির্দিষ্ট GID দিয়ে group তৈরি
    # -g → GID (group ID) নির্দিষ্ট করার জন্য

    👉 sudo groupdel developers                # group মুছে ফেলা
    👉 sudo groupmod -n dev_team developers    # group name rename | -n = নতুন নাম

    # User কে group এ assign করা

    👉 sudo usermod -g <group-name> <username>         # assign group
    👉 sudo usermod -a -G designers,qa soruov          # -a = append

    👉 groups <username>                               # All assign group list

    👉 cat /etc/group                                  # All Groups
    👉 cut -d: -f1 /etc/group                          # শুধু নামগুলো দেখতে
```
### 🧩 User Manage

```bash
    👉 sudo useradd soruov                     # Normal user create
    👉 sudo useradd -m soruov                  # User create with home directory
    👉 sudo useradd -m -s /bin/bash -g developers soruov   # home directory + shell + group নির্দিষ্ট করে
    👉 sudo useradd -u 1002 -m soruov          # Specific UID নির্দিষ্ট করে

    👉 sudo userdel soruov                     # শুধু user মুছে দেয়
    👉 sudo userdel -r soruov                  # user + home directory মুছে দেয়

    👉 sudo usermod -l newname oldname             # username rename
    👉 sudo usermod -d /home/newname -m newname    # home directory rename করতে

    👉 sudo passwd soruov                          # user password set/change
    👉 sudo passwd -e soruov                       # password expire করানো
    👉 sudo passwd -l soruov                       # password disable করা
    👉 sudo passwd -u soruov                       # password enable করা

    👉 id soruov                                   # User Info
    👉 getent passwd soruov                        # home directory, shell, etc.

    👉 sudo usermod -L soruov                      # lock user (login blocked)
    👉 sudo usermod -U soruov                      # unlock user

```
📌 -d + -m → old home directory move করা হয়।\
📌 -m → home directory তৈরি করবে\
📌 -s → login shell নির্দিষ্ট করবে\
📌 -g → primary group\
📌 -G → secondary group(s)


### 🧩 File & Folder Permission

```bash
    👉 chmod u+x script.sh             # owner কে execute যোগ করা (<owner>+<execute = x>)
    👉 chmod g-w file.txt              # group থেকে write permission remove করা (<group>+<write = w>)
    👉 chmod a+r file.txt              # all users কে read যোগ করা (owner, group, other) (<all> + <read = r>)


    👉 chmod 755 script.sh
    # owner: rwx (4+2+1=7)
    # group: r-x (4+0+1=5)
    # others: r-x (4+0+1=5)

    👉 chmod 644 file.txt
    # owner: rw- (4+2=6)
    # group: r-- (4)
    # others: r-- (4)

    👉 chmod -R 755 /project                       # সব ফাইল ও ফোল্ডারের permission পরিবর্তন

    👉 chown soruov file.txt                       # Owner পরিবর্তন
    👉 chown soruov: file.txt                      # Owner পরিবর্তন
    👉 chown :developers file.txt                  # Group পরিবর্তন
    👉 chown soruov:developers file.txt            # Owner + Group পরিবর্তন
    👉 chown -R soruov:developers /var/www/html    # recursive সব ফোল্ডারের জন্য

    # শুধুমাত্র গ্রুপ পরিবর্তন করতে ব্যবহার হয়।

    👉 chgrp developers file.txt
    👉 chgrp -R staff /project

```
#### 📌 Symbol & Meaning

| Symbol | Meaning                   | Description                                    |
|--------|---------------------------|------------------------------------------------|
| u      | user / owner             | The owner of the file                          |
| g      | group                    | Users belonging to the file's group           |
| o      | others                   | Everyone else                                 |
| a      | all                      | All of the above (user, group, others)        |

### 🧩 Network

```bash
👉 hostname                             # Hostname দেখায়
👉 hostname -I                          # Hostname IP দেখা যায়
👉 ifconfig                             # ইন্টারফেসের IP, MAC, RX/TX stats দেখায়
👉 ip addr show / ip a                  # IP এবং state দেখা যায়
👉 ip link show / ip l                  # status দেখা যায় (UP/DOWN, MAC)
👉 ip route show / ip r                 # রাউটিং টেবিল দেখায়
👉 nmcli device status                  # রাউটিং টেবিল দেখায়

# Network Testing / Troubleshooting

👉 ping 8.8.8.8                        # Network connectivity test
👉 ping google.com                     # DNS test
👉 traceroute google.com               # প্যাকেট কোন রাউট দিয়ে যাচ্ছে তা দেখা যায়
👉 mtr google.com                      # ping + traceroute একসাথে (real-time)
👉 curl -I http://example.com          # HTTP header response দেখা যায়
👉 wget http://example.com             # URL থেকে ডাটা ডাউনলোড করার জন্য
```
### 🧩 Firewall / Security
```bash
👉 sudo ufw status                      # Firewall এর বর্তমান status দেখা যায়
👉 sudo ufw enable                         # Firewall চালু করা
👉 sudo ufw disable                        # Firewall বন্ধ করা

# Reset / Reload

👉 sudo ufw reload
👉 sudo ufw reset 

# Allow / Deny Rules

👉 sudo ufw allow 22
👉 sudo ufw allow 80/tcp
👉 sudo ufw deny 23
👉 sudo ufw reject 25

# Delete / Modify Rules

👉 sudo ufw delete allow 22
👉 sudo ufw delete deny 23

```











