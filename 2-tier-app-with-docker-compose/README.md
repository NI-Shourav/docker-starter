<div align="center">

# 🚀 2-Tier Application with Docker Compose

An elegant, containerized 2-tier architecture featuring a Python Flask backend and a MySQL database. Handled effortlessly with Docker Compose.

[![Docker](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Docker Compose](https://img.shields.io/badge/Docker_Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/compose/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![AWS EC2](https://img.shields.io/badge/Amazon_EC2-FF9900?style=for-the-badge&logo=amazon-ec2&logoColor=white)](https://aws.amazon.com/ec2/)

</div>

---

## ☁️ EC2 Deployment Guide

Deploying this application to AWS EC2 is straightforward. Follow these steps to get your environment up and running!

### 1️⃣ Provision an EC2 Instance
First, launch an EC2 instance (e.g., Ubuntu) in your [AWS Management Console](https://console.aws.amazon.com/).

### 2️⃣ System Update
SSH into your instance and ensure all packages are up to date:
```bash
sudo apt update
```

### 3️⃣ Install Docker
Install the Docker engine:
```bash
sudo apt install docker.io -y
```

### 4️⃣ Grant Docker Permissions
Avoid typing `sudo` every time! Add your user to the `docker` group:
```bash
sudo usermod -aG docker $USER
```

### 5️⃣ System Reboot
Reboot to apply the new group permissions:
```bash
sudo reboot
```

### 6️⃣ Fetch the Project
Once reconnected, clone this repository (if not already done) and bring up the project directory:
```bash
# git clone <repository-url>
cd 2-tier-app-with-docker-compose
```

### 7️⃣ Install Docker Compose
Download the latest Docker Compose binary as a CLI plugin and grant execution rights:
```bash
sudo mkdir -p /usr/local/lib/docker/cli-plugins
sudo curl -SL "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/lib/docker/cli-plugins/docker-compose && \
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
```

### 8️⃣ Verify Installation
Check if Docker Compose was installed successfully:
```bash
docker compose version
```
> **Expected Output:** `Docker Compose version v2.x.x`

### 9️⃣ Launch the App! 🚀
Fire up the backend and database containers in detached mode:
```bash
docker compose up -d
```

### 🔟 Configure Security Group
Ensure the world can see your app! In the **AWS EC2 Console**:
- Select your instance > **Security** > **Security Groups**.
- Edit inbound rules: Add **Custom TCP** on port `5000` with source `0.0.0.0/0`.

### 🌐 Access Your Application
Open your favorite browser and visit your EC2's public IP address:
```
http://<EC2-PUBLIC-IP>:5000
```

### 💾 Verify Data Persistence
Test if your database safely stores data across restarts:

1. Stop the containers:
   ```bash
   docker compose down
   ```
2. Start them up again:
   ```bash
   docker compose up -d
   ```
3. Check the application in your browser—*all your saved data will remain intact!*

---

## 🧹 Stopping and Cleaning Up

Depending on your needs, you can easily halt or reset your environment.

### Stop Containers Safely
Stop the background containers and remove created networks:
```bash
docker compose down
```

### ⚠️ Full Nuclear Cleanup
Stop and remove **everything**, including containers, networks, database volumes (🚨 **this destroys data permanently**), and built images:
```bash
docker compose down --volumes --rmi all
```

---

## 📂 Project Structure

A quick glance at what makes this application tick:

| File/Directory | Description |
|----------------|-------------|
| 🐳 `Dockerfile` | Multi-stage build magic to craft a lean Flask Docker image. |
| 🛠️ `compose.yml` | Multi-container blueprint for orchestrating MySQL & Flask. |
| 📦 `requirements.txt` | Python dependencies. |
| 🚀 `run.py` | The main entry point to ignite the Flask backend. |
| 📁 `app/` | Home to the core Flask application source code. |

---
<div align="center">
  <i>Built with ❤️ using Docker Compose</i>
</div>
