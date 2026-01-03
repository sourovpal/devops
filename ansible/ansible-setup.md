# Ansible Install & Setup

### 🧩 Step 1: Install

```bash
  sudo apt update
  sudo apt install -y ansible

  ansible --version
```

### 🧩 Step 2: Inventory file configer
inventory.ini

```bash
  [web]
  192.168.1.10
  192.168.1.11
  
  [db]
  192.168.1.20
  
  [all:vars]
  ansible_user=ubuntu
  ansible_ssh_private_key_file=~/.ssh/id_rsa

  # Ping Test
  ansible all -i inventory.ini -m ping

```

### 🧩 Install Nginx

nginx.yml

```bash
  - name: Install and start Nginx
    hosts: web
    become: yes
  
    tasks:
      - name: Update apt
        apt:
          update_cache: yes
  
      - name: Install nginx
        apt:
          name: nginx
          state: present
  
      - name: Start nginx
        service:
          name: nginx
          state: started
          enabled: yes

  # Run: ansible-playbook -i inventory.ini nginx.yml

```

### 🧩 Copy file to servers

```bash
  - name: Copy index.html
    hosts: web
    become: yes
  
    tasks:
      - name: Copy file
        copy:
          src: index.html
          dest: /var/www/html/index.html

```

### 🧩 Run Linux command

```bash
  - name: Run shell command
  hosts: all

  tasks:
    - name: Check disk space
      command: df -h
      register: disk

    - debug:
        var: disk.stdout

```

### 🧩 Conditional (if else)

```bash 
  - name: Install based on OS
  hosts: all
  become: yes

  tasks:
    - name: Install nginx on Ubuntu
      apt:
        name: nginx
        state: present
      when: ansible_os_family == "Debian"

```

### 🧩 Loop

```bash 
  - name: Install common packages
    hosts: all
    become: yes
  
    tasks:
      - name: Install packages
        apt:
          name: "{{ item }}"
          state: present
        loop:
          - git
          - curl
          - unzip

```



