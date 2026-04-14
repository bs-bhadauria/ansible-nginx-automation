# Automated Infrastructure Configuration using Ansible on AWS

## Project Overview

This project demonstrates how to automate infrastructure configuration using **Ansible** on **AWS EC2 instances**.
The objective is to eliminate manual server management by implementing automated, repeatable, and idempotent configurations.

---

## Architecture

```
Local Machine (You)
        ↓
Control Node (EC2 - Ansible Installed)
        ↓ (SSH - Key Based Authentication)
Managed Node (EC2 - Target Server)
```

* **Control Node**: Runs Ansible
* **Managed Node**: Target machine where tasks are executed
* **Communication**: Secure SSH (Passwordless Authentication)

---

## Key Features

* 🔐 Passwordless SSH Authentication
* 📦 Automated Nginx Installation
* ⚡ Ad-hoc Command Execution
* 📄 YAML-based Playbook Automation
* 🔁 Idempotent Configuration Management
* 🛠 Real-world Debugging (SSH + YAML + Infra)

---

## Technologies Used

* **Ansible**
* **AWS EC2**
* **Linux (Ubuntu)**
* **SSH (Secure Shell)**
* **YAML**

---

## 🔑 Step 1: SSH Key-Based Authentication

Generate SSH key on Control Node:

```bash
ssh-keygen
```

Copy public key to Managed Node:

```bash
nano ~/.ssh/authorized_keys
```

Set permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

## 📂 Step 2: Inventory File

Create `inventory` file:

```ini
[web]
managed ansible_host=15.206.82.151 ansible_user=ubuntu ansible_ssh_private_key_file=/home/ubuntu/.ssh/id_ed25519
```

---

## ⚡ Step 3: Ad-hoc Commands

Test connectivity:

```bash
ansible -i inventory web -m ping
```

Run remote command:

```bash
ansible -i inventory web -m command -a "uptime"
```

---

## 📄 Step 4: Ansible Playbook

Create `playbook.yml`:

```yaml
---
- name: Install Nginx
  hosts: all
  become: true

  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Start Nginx
      service:
        name: nginx
        state: started
```

---

## ▶️ Step 5: Run Playbook

```bash
ansible-playbook -i inventory playbook.yml
```

---

## ✅ Output

```
PLAY RECAP
managed : ok=3 changed=0 unreachable=0 failed=0
```

---

## Verification

Open in browser:

```
http://<Managed_Node_Public_IP>
```

You should see the **Nginx Welcome Page**.

---

## Key Learnings

* Infrastructure automation using Ansible
* SSH key-based authentication setup
* Inventory management
* YAML syntax and debugging
* Idempotency in automation
* Remote execution without manual SSH
* Real-world troubleshooting (network, SSH, permissions)

---

## ⚠️ Errors Faced & Fixes

### ❌ Host key verification failed

```bash
ssh-keygen -R <ip>
```

---

### ❌ Server refused our key

**Fix:** Specify correct private key in inventory

---

### ❌ YAML Syntax Error

* Cause: Wrong indentation
* Fix: Use proper spacing (2 spaces only, no tabs)

---

## Future Enhancements

* 🔁 Ansible Roles implementation
* ☁️ Dynamic Inventory (AWS)
* 🔄 CI/CD Integration (Jenkins/GitHub Actions)
* 📦 Multi-server orchestration
* 🔐 Advanced security (IAM roles, bastion host)

---

## 📸 Screenshots

*Add screenshots of:*

* Successful playbook execution
* Nginx running in browser

---

## Conclusion

This project demonstrates a foundational DevOps workflow by combining:

* Cloud Infrastructure (AWS)
* Configuration Management (Ansible)
* Automation Principles

It showcases the transition from **manual server management → automated infrastructure provisioning**.

---

## Author

**Bhoopendra Singh Bhadauria**
Aspiring DevOps Engineer 🚀

---

## Connect With Me

* LinkedIn: https://www.linkedin.com/in/bhoopendra-singh-bhadauria/
* GitHub: https://github.com/bs-bhadauria

---

## If you found this useful, give this repo a star!
