# Linux Commands

### 🧩 File & Folder Command

```bash
    👉 pwd                             # বর্তমান ডিরেক্টরির পথ দেখায়
    👉 ls                              # ডিরেক্টরির ফাইল/ফোল্ডার দেখায়
    👉 ls -l                           # ডিরেক্টরির ফাইল/ফোল্ডার দেখায়, ডিটেইলড লিস্ট দেখায় (পারমিশন, Owner, সাইজ)
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


### 🧩 Install Package
```bash
    👉 sudo apt update                    # repository থেকে লেটেস্ট প্যাকেজ list download করে
    👉 sudo apt upgrade -y                # সব install হওয়া package লেটেস্ট version এ update করে
    # -y flag: auto yes করে prompt skip করে।

    👉 sudo apt install <package-name> -y        # Package Install command
    👉 sudo apt install git curl wget -y         # Example Install Package
    👉 sudo apt --fix-broken install             # Fix broken dependencies

    👉 sudo apt remove <package-name> -y         # Remove Package
    👉 sudo apt purge <package-name> -y          # Package + configuration সম্পূর্ণ remove করে
    👉 apt search <package-name>                 # Package Search
    👉 apt show <package-name>                   # Show package info

    # Clean / Autoremove

    👉 sudo apt autoremove -y
    👉 sudo apt clean
    👉 sudo apt autoclean

    # autoremove: unused dependencies remove করে।
    # clean: downloaded package archive remove করে।
    # autoclean: পুরনো/archive packages remove করে।
```

### 🧩 Linux Folder Structure Explanation

| Directory | Description |
|----------|-------------|
| `~`      | Login User Home Directory |
| `/`      | Root directory. The top-level directory of the entire filesystem |
| `/bin`  | Essential user command binaries (e.g., `ls`, `cp`, `mv`) |
| `/boot` | Bootloader files, Linux kernel, initramfs |
| `/dev`  | Device files (hard disks, USB, terminals, etc.) |
| `/etc`  | System-wide configuration files |
| `/home` | Home directories for regular users |
| `/lib`  | Essential shared libraries for `/bin` and `/sbin` |
| `/lib64`| 64-bit shared libraries |
| `/media`| Mount point for removable media (USB, CD-ROM) |
| `/mnt`  | Temporary mount point for filesystems |
| `/opt`  | Optional or third-party software packages |
| `/proc` | Virtual filesystem with system & process info |
| `/root` | Home directory for the root user |
| `/run`  | Runtime data (PID files, sockets) |
| `/sbin` | System binaries for administration |
| `/srv`  | Data for services (web, FTP, etc.) |
| `/sys`  | Kernel and hardware information |
| `/tmp`  | Temporary files (cleared on reboot) |
| `/usr`  | User applications and utilities |
| `/usr/bin` | User command binaries |
| `/usr/sbin` | System admin binaries |
| `/usr/lib` | Libraries for `/usr/bin` and `/usr/sbin` |
| `/var`  | Variable data (logs, cache, mail, spool) |
| `/var/log` | System and application log files |
| `/var/tmp` | Temporary files kept between reboots |

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

### 🧩 Group Manage

```bash
    👉 sudo groupadd developers            # নতুন group তৈরি
    👉 sudo groupadd -g 1001 designers     # নির্দিষ্ট GID দিয়ে group তৈরি
    # -g → GID (group ID) নির্দিষ্ট করার জন্য

    👉 sudo groupdel developers                # group মুছে ফেলা
    👉 sudo groupmod -n dev_team developers    # group name rename | -n = নতুন নাম

    # User কে group এ assign করা

    👉 sudo usermod -g <group-name> <username>         # assign group
    👉 sudo usermod -a -G designers,qa soruov          # -a = append
    👉 sudo usermod -aG designers,qa soruov            # -a = append

    👉 groups <username>                               # All assign group list

    👉 cat /etc/group                                  # All Groups
    👉 cut -d: -f1 /etc/group                          # শুধু নামগুলো দেখতে
```
### 🧩 User Manage

```bash
    👉 su - / sudo -i                # root user এ switch করে
    👉 exit / logout                 # current user logout করে

    👉 sudo useradd soruov                     # Normal user create
    👉 sudo useradd -m soruov                  # User create with home directory
    👉 sudo useradd -m -s /bin/bash -g developers soruov   # home directory + shell + group নির্দিষ্ট করে
    👉 sudo useradd -u 1002 -m soruov          # Specific UID নির্দিষ্ট করে

    👉 sudo chsh -s /bin/bash username
    👉 chsh -s /bin/bash                        # নিজের user হলে

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

#### 📌 Permission with numeric value

| Permission | Numeric Value |
|-----------------|---------------|
| ---             | 0             |
| --x             | 1             |
| -w-             | 2             |
| -wx             | 3             |
| r--             | 4             |
| r-x             | 5             |
| rw-             | 6             |
| rwx             | 7             |

### 🧩 Network

```bash
👉 hostname                             # Hostname দেখায় (login username)
👉 hostname -I                          # Hostname IP দেখা যায়
👉 ifconfig                             # ইন্টারফেসের IP, MAC, RX/TX stats দেখায়
👉 ip addr show / ip a                  # IP এবং state দেখা যায়
👉 ip link show / ip l                  # status দেখা যায় (UP/DOWN, MAC)
👉 ip route show / ip r                 # রাউটিং টেবিল দেখায়
👉 nmcli device status                  # রাউটিং টেবিল দেখায়

# Network Testing / Troubleshooting

👉 ping 8.8.8.8                        # Network connectivity test
👉 ping google.com                     # DNS test
👉 ping -c 4 google.com                # -c (count) – কয়টা packet পাঠাবে
👉 ping -i 2 google.com                # -i (interval) – packet পাঠানোর সময় ব্যবধানে
👉 ping -s 100 google.com              # -s (size) – packet size (byte)



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

### 🧩 SSH (Secure Shell)

```bash

    👉 sudo apt update
    👉 sudo apt install openssh-client -y
    👉 sudo apt install openssh-server -y

    👉 sudo systemctl status ssh
    👉 sudo systemctl start ssh
    👉 sudo systemctl stop ssh
    👉 sudo systemctl restart ssh
    👉 sudo systemctl enable ssh
    👉 sudo nano /etc/ssh/sshd_config
    
    # Generate SSH Key - path = ~/.ssh (Server SSH)

    👉 ssh-keygen                        # Generate ssh key
    👉 ssh-keygen -t rsa -b 4096         # key generage with rsa for remote access
    👉 ssh-keygen -t ed25519
    👉 authorize_keys                    # Copy id_rsa.pub then paste this file
    👉 cat ~/.ssh/id_rsa.pub | ssh user@host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"     # Manual copy
    
    # Remote server login system (Client SSH)

    👉 ssh -i <private key> <user>@<ip> bash/sh
    👉 ssh -i ~/.ssh/id_rsa sourov@123.12.12.1 bash/sh

    # SSH Config File (~/.ssh/config)
    
    Host myserver
    HostName 192.168.1.10
    User root
    Port 2222
    IdentityFile ~/.ssh/id_rsa

    👉 ssh myserver         # use

    # File Transfer (SCP / RSYNC)

    👉 scp file.txt user@host:/path
    👉 scp user@host:/path/file.txt .
    👉 scp -r folder user@host:/path
    👉 scp -P 2222 file.txt user@host:/path
    
    👉 rsync -avz file.txt user@host:/path
    👉 rsync -avz --progress folder user@host:/path
    👉 rsync -avz -e "ssh -p 2222" folder user@host:/path
```
📌 -t  = Key type\
📌 -b = Bit size\
📌 -C = Comment

### 🧩 Wget & Download file
```bash
    sudo apt update
    sudo apt install wget -y

    wget -c https://example.com/bigfile.zip
    0 2 * * * wget -O /home/user/file.zip https://example.com/file.zip     # auto download
    wget -qO- https://example.com/file.txt | grep "keyword"
```
#### 📌 Download Command Options

| Option          | Description                       | Example |
|-----------------|-----------------------------------|---------|
| `-O`            | Save as custom filename           | `wget -O myfile.html https://example.com` |
| `-c`            | Resume download                   | `wget -c https://example.com/largefile.zip` |
| `-b`            | Background download               | `wget -b https://example.com/file.zip` |
| `-i file`       | Download URLs from a file         | `wget -i urls.txt` |
| `-r`            | Recursive download                | `wget -r https://example.com/folder/` |
| `-np`           | No parent directory               | `wget -r -np https://example.com/folder/` |
| `-k`            | Convert links for local browsing  | `wget -r -k https://example.com` |
| `--limit-rate`  | Limit bandwidth                   | `wget --limit-rate=200k https://example.com/file.zip` |
| `--user-agent`  | Set custom user-agent             | `wget --user-agent="Mozilla/5.0" https://example.com` |
| `--header`      | Add HTTP headers                  | `wget --header="Authorization: Bearer TOKEN" https://example.com` |
| `-q`            | Quiet mode                        | `wget -q https://example.com/file.zip` |
| `-v`            | Verbose mode                      | `wget -v https://example.com/file.zip` |











