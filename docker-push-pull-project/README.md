<div align="center">
  <h1>🐳 Docker Push-Pull Workflow Guide</h1>
  <p>A comprehensive step-by-step guide for deploying Node.js applications with Docker on AWS EC2, covering both default and custom bridge networks.</p>

  <img src="https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/Amazon_AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white" />
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" />
  <img src="https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" />

  <br />
</div>

---

## 📑 Table of Contents
- [Prerequisites & Server Setup](#-prerequisites--server-setup)
- [General Deployment Steps](#-general-deployment-steps)
- [Running the Container](#-running-the-container)
  - [Method 1: Default Bridge Network](#method-1-default-bridge-network)
  - [Method 2: Custom Bridge Network](#method-2-custom-bridge-network)
- [Cleanup Commands](#-cleanup-commands)

---

## 🛠 Prerequisites & Server Setup

To get started, spin up an **AWS EC2 Instance** (Ubuntu is recommended) and connect to it. Once you are connected, follow these steps to prepare your environment.

**1. Update System Packages**
Keep your system secure and up-to-date.
```bash
sudo apt update && sudo apt upgrade -y
```

**2. Install Docker**
Install the Docker daemon to containerize applications.
```bash
sudo apt install docker.io -y
```

**3. Install Node.js & npm**
Install the JavaScript runtime and package manager.
```bash
sudo apt install nodejs npm -y
```

**4. Grant Docker Permissions**
Add your user to the Docker group so you don't need to type `sudo` for every Docker command.
```bash
sudo usermod -aG docker $USER
```

**5. Reboot System**
Reboot to ensure the group changes and updates take effect.
```bash
sudo reboot
```

---

## 🚀 General Deployment Steps

Follow these steps to build and push your Docker image.

**1. Clone the Repository**
```bash
git clone <repository_url>
cd docker-push-pull-project
```

**2. Build the Docker Image**
Make sure you are in the directory containing your `Dockerfile`.
```bash
docker build -t <YOUR_DOCKERHUB_ID>/<image_name>:<tag> .
```

**3. Login to Docker Hub**
Generate a **Personal Access Token (PAT)** in your Docker Hub account settings. Use it as your password when prompted!
```bash
docker login -u <YOUR_DOCKERHUB_ID>
```

**4. Push the Image**
Push the newly built image up to your Docker Hub repository.
```bash
docker push <YOUR_DOCKERHUB_ID>/<image_name>:<tag>
```

**5. Clean Up Local Image (Optional but recommended)**
Remove the local copy to verify the upcoming `run` command properly pulls it from Docker Hub.
```bash
docker rmi <IMAGE_ID>
```

---

## 🚢 Running the Container

Once your image is built and pushed to Docker Hub, you can run it using either the **Default Bridge Network** or a **Custom Bridge Network**. Choose the method that best fits your needs.

### Method 1: Default Bridge Network

The simplest way to run your container.

**1. Run the Container**
The `--rm` flag removes the container once it stops. The `-p 3000:8080` maps your host machine's port 3000 to the container's port 8080.
```bash
docker run --rm -p 3000:8080 <YOUR_DOCKERHUB_ID>/<image_name>:<tag>
```

> **Note on Accessing the App:**
> 1. Go to your EC2 console > **Security Groups**.
> 2. Add an **Inbound Rule** for Custom TCP on port `3000`.
> 3. Open your browser and navigate to `http://<ec2_public_ip>:3000`.
> 
> *To stop the container running in the foreground, press `Ctrl+C` (or `Ctrl+Z` to suspend).*

### Method 2: Custom Bridge Network

Using a custom bridge network provides better isolation and automatic DNS resolution between containers.

**1. Create a Custom Network**
```bash
docker network create <network_name>
```

**2. Verify Network Creation**
List all networks to ensure yours was created successfully.
```bash
docker network ls
```

**3. Run Container on Custom Network**
Attach the `--network` flag to specify your custom bridge network.
```bash
docker run --rm --network <network_name> -p 3000:8080 <YOUR_DOCKERHUB_ID>/<image_name>:<tag>
```

**4. Inspect Network Details (Troubleshooting)**
Verify the container's internal IP and network bindings.
```bash
docker inspect <container_id>
```

> **Note on Accessing the App:**
> Keep the same **Inbound Rule** for port `3000` on your EC2 Security Group, and navigate to `http://<ec2_public_ip>:3000` to access your app.

---

## 🧹 Cleanup Commands

Commands to help you maintain a clean Docker environment on your server.

**Stop a Running Container**
```bash
docker stop <container_id>
```

**Remove All Stopped Containers**
```bash
docker container prune -a
```

**Remove All Unused Images**
```bash
docker image prune -a
```

<br/>
<div align="center">
  <i>Happy Containerizing! 🚢</i>
</div>
