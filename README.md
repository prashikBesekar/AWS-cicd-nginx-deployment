# 🚀 AWS CI/CD Deployment with Nginx (Production-Style Setup)

![AWS](https://img.shields.io/badge/AWS-EC2-orange?style=for-the-badge&logo=amazon-aws)
![Nginx](https://img.shields.io/badge/Nginx-Server-green?style=for-the-badge&logo=nginx)
![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub_Actions-blue?style=for-the-badge&logo=github-actions)
![Status](https://img.shields.io/badge/Status-Live-brightgreen?style=for-the-badge)

---

## 📌 Project Overview

A **production-style CI/CD pipeline** that automatically deploys a web application to an **AWS EC2 instance** using **GitHub Actions** and **Nginx** as a reverse proxy server.

Every time code is pushed to the `main` branch — the pipeline automatically pulls the latest code, builds, and serves it live on the internet. Zero manual deployment needed.

> 💡 This project simulates a real-world DevOps workflow used by companies to deploy applications at scale.

---

## ⚙️ Tech Stack

| Technology | Purpose |
|---|---|
| **AWS EC2** | Cloud server to host the application |
| **Nginx** | Reverse proxy + web server |
| **GitHub Actions** | CI/CD pipeline automation |
| **Git** | Version control and deployment trigger |
| **AWS Security Groups** | Firewall and port management |
| **SSH** | Secure server access and deployment |

---

## 🏗️ Architecture

```
Developer pushes code
        ↓
   GitHub Repository
        ↓
  GitHub Actions triggers
        ↓
  SSH into AWS EC2
        ↓
  Pull latest code
        ↓
  Nginx serves updated app
        ↓
  Live on Public IP 🌐
```

---

## 🔧 What I Built & Solved

### ✅ Nginx Configuration
- Configured custom Nginx **server blocks** to serve the application on port 80
- Set up proper **root directory**, index file routing, and error handling
- Enabled Nginx to start automatically on server reboot

### ✅ GitHub Actions CI/CD Pipeline
- Created a workflow that **triggers automatically on every push** to `main` branch
- Pipeline SSHs into EC2, pulls latest code, and restarts Nginx — all without manual steps
- Implemented **seamless code deployment** from GitHub directly to production server

### ✅ AWS EC2 Setup & Security
- Launched and configured an EC2 instance (Ubuntu)
- Configured **AWS Security Groups** to allow HTTP (port 80), HTTPS (port 443), and SSH (port 22)
- Resolved production issues including **port conflicts**, **file permission errors**, and **server failures**

### ✅ Real Production Problems Solved
- Fixed Nginx configuration errors causing 502 Bad Gateway
- Resolved SSH key authentication issues in GitHub Actions secrets
- Debugged and fixed server permission conflicts during deployment

---

## 🚀 How to Run This Project

### Prerequisites
- AWS Account with EC2 access
- GitHub Account
- Basic knowledge of Linux/Ubuntu commands

### Step 1 — Launch EC2 Instance
```bash
# Launch Ubuntu EC2 instance on AWS Console
# Allow ports: 22 (SSH), 80 (HTTP), 443 (HTTPS) in Security Groups
```

### Step 2 — Install Nginx on EC2
```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

### Step 3 — Configure Nginx Server Block
```bash
sudo nano /etc/nginx/sites-available/myapp

# Add this configuration:
server {
    listen 80;
    server_name your-ec2-public-ip;

    root /var/www/myapp;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

# Enable the config
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### Step 4 — Set Up GitHub Actions
```yaml
# .github/workflows/deploy.yml
name: Deploy to AWS EC2

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to EC2
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ubuntu
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            cd /var/www/myapp
            git pull origin main
            sudo systemctl reload nginx
```

### Step 5 — Add GitHub Secrets
Go to your GitHub repo → Settings → Secrets and add:
- `EC2_HOST` — Your EC2 public IP address
- `EC2_SSH_KEY` — Your EC2 private key (.pem file content)

---

## 📸 Key Learnings

- How **CI/CD pipelines** work in real production environments
- How to configure **Nginx as a reverse proxy** and web server
- How to manage **AWS EC2 instances** and security groups
- How to use **GitHub Actions** for automated deployments
- Debugging real production issues like port conflicts and permission errors

---

## 🔗 Connect With Me

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Prashik_Besekar-blue?style=flat&logo=linkedin)](https://linkedin.com/in/prashik-besekar)
[![GitHub](https://img.shields.io/badge/GitHub-prashikBesekar-black?style=flat&logo=github)](https://github.com/prashikBesekar)

---

⭐ **If this project helped you, please give it a star!**
