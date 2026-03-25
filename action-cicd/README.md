<div align="center">

# 🚀 Automated CI/CD Deployment with GitHub Actions

[![GitHub Actions Status](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)](#)
[![Docker Support](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](#)
[![AWS EC2](https://img.shields.io/badge/AWS_EC2-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)](#)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)](#)

*A seamless demonstration of Continuous Integration and Deployment, pushing Dockerized applications directly to AWS EC2.*

**[Prerequisites](#-prerequisites) • [Setup Guide](#️-step-by-step-setup) • [Trigger Deployment](#-trigger-deployment)**

</div>

---

## 📋 Prerequisites

Before diving in, you'll need an AWS EC2 instance. The AWS **Free Tier** is more than enough for this setup!

> **💡 TIP:** Make sure you select an **Ubuntu** AMI for maximum compatibility with this guide.

<details>
<summary><b>🔥 Click here to view Security Group Requirements</b></summary>
When launching your instance, ensure your Security Group has the following inbound rules enabled:

- `Port 22` — SSH Access
- `Port 80` — HTTP Traffic
- `Port 443` — HTTPS Traffic
- `Port 5000` — Application Port
</details>

---

## �️ Step-by-Step Setup

### Step 1: 🐳 Prepare the EC2 Instance

Connect to your EC2 virtual machine using SSH and run the following commands to install and configure Docker.

```bash
# 1. Update your package manager
sudo apt update -y

# 2. Install the Docker Engine
sudo apt install -y docker.io

# 3. Grant your current user Docker permissions (removes the need for 'sudo')
sudo usermod -aG docker $USER

# 4. Reboot your machine to apply group changes
sudo reboot
```

> **Note:** After your machine restarts, reconnect via SSH and verify everything is working smoothly by running `sudo systemctl status docker` and `docker ps`.

### Step 2: 🔑 Configure GitHub Secrets

For GitHub Actions to securely access your EC2 environment and pull images from Docker Hub, you must store credentials as repository secrets.

Navigate to **Settings** > **Secrets and variables** > **Actions** > **New repository secret** and add the following:

| Secret Key | What goes here? |
| :--- | :--- |
| `AWS_HOST` | The public IP Address of your EC2 instance |
| `AWS_KEY` | The full, raw text from your downloaded `.pem` key file |
| `AWS_USER` | The default SSH user (e.g., `ubuntu`) |
| `DOCKER_USERNAME` | Your Docker Hub Username |
| `DOCKER_PASSWORD` | Your Docker Hub Password *(or PAT)* |

### Step 3: ⚙️ Setup GitHub Workflow

Create a workflow file in your repository under `.github/workflows/main.yaml`. 
*(Simply copy the deployment configuration from `action.yml` into this file!)*

---

## 🚀 Trigger Deployment

With everything in place, the CI/CD magic happens automatically!

1. Edit any code file within your application.
2. `git commit` and `git push` to your main branch.
3. Automatically watch the deployment run successfully in the **Actions** tab on GitHub! ✨

---

## 🎉 View Your Project

Once the pipeline successfully finishes, your app is fully containerized and hosted live.

👉 Open your browser and navigate to:
```text
http://<YOUR_EC2_IP>:5000
```
*Enjoy your automated infrastructure!*
