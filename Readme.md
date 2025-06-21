# 🤖 Automated NGINX Installation on AWS EC2 using Ansible

Welcome to the infra-automation dojo 🥷  
In this project, we automate the setup of an NGINX web server on an AWS EC2 instance using **Ansible**. No more manual installs — just clean, repeatable, scalable infrastructure-as-code.

---

## 📦 Project Overview

We’re automating the installation of NGINX (a web server) on a cloud-hosted EC2 machine using Ansible, because manual setup is for peasants 😤  
We're ruling the infra kingdom with automation. Here's how:

---

## 🪜 Step-by-Step Guide

### 1️⃣ Launch Your EC2 Instance

**What we did:**
- Spun up an **Amazon EC2** instance.
- Selected **Amazon Linux 2** or **Ubuntu**.
- Downloaded the `.pem` key file for SSH access.

**Why:**
The EC2 instance is our **target machine** — the remote server we want to configure.  
The `.pem` key proves our identity and grants secure access via SSH.

---

### 2️⃣ Install Ansible on the Controller Machine

**What we did:**
- Installed Ansible on our **local machine** or **controller node**.
- Verified installation with:

```bash
ansible --version
````

**Why:**
Ansible runs from a control machine. It connects to EC2 over SSH, sends instructions (via a playbook), and automates everything.

---

### 3️⃣ Set Up the Inventory File (`inventory.ini`)

**Create the file:**

```ini
[webserver]
your-ec2-ip ansible_user=ec2-user ansible_ssh_private_key_file=your-key.pem
```

**Explanation:**

* `[webserver]` is a group name for your EC2 host.
* `ansible_user` should match your EC2 image (`ec2-user` for Amazon Linux, `ubuntu` for Ubuntu).
* `ansible_ssh_private_key_file` is the path to your downloaded `.pem` file.

Ansible needs to know where to go and how to log in.
The `[webserver]` is a group name (so we can target it).
`ansible_user=ec2-user` tells it which SSH user to use.
`ansible_ssh_private_key_file=your-key.pem` gives it SSH access without needing a password.

🧠 Basically:
“Ansible, here’s your VIP access to the EC2 party.” 🎫

---

### 4️⃣ Write the Ansible Playbook (`install_nginx.yml`)

```yaml
- name: Install NGINX on target EC2
  hosts: "{{ target }}"
  become: yes
  tasks:
    - name: Install NGINX
      package:
        name: nginx
        state: present

    - name: Start and enable NGINX service
      service:
        name: nginx
        state: started
        enabled: true
```

**What’s happening here:**

* `hosts: "{{ target }}"`: allows dynamic group targeting (like `webserver`).
* `become: yes`: uses sudo/root access.
* The first task installs NGINX.
* The second ensures it starts now and on every boot.

This is like:
“Yo EC2, get NGINX up, keep it running, and do it forever, alright?”

---

### 5️⃣ Run the Playbook

```bash
ansible-playbook -i inventory.ini install_nginx.yml -e "target=webserver"
```

**What this does:**

* `-i inventory.ini`: points to your host list.
* `-e "target=webserver"`: tells the playbook which group to act on.

Ansible connects to the EC2 instance, runs the playbook, and exits cleanly. 🧼

---

### 6️⃣ Verify the Setup

* Open your browser.
* Go to:

  ```http
  http://your-ec2-public-ip
  ```
* You should see the default **NGINX welcome page**.

If it works, you’ve successfully:
✅ Connected
✅ Installed
✅ Configured
✅ Verified — all via automation 🚀

----

## 🧠 Concepts Covered

* Cloud provisioning (EC2)
* SSH-based authentication
* Ansible inventory and playbook structure
* Package installation and service management
* Infrastructure-as-Code principles

---


Infrastructure automation saves time, prevents errors, and keeps your deployment **repeatable and scalable**.
Congrats, you just took your first step toward becoming a DevOps wizard 🧙‍♂️

