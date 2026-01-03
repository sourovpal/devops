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




