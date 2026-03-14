<div align="center">
  <h1>🚀 Dockerized Flask App on AWS EC2 🚀</h1>
  <p><i>A comprehensive, step-by-step guide to deploying a Dockerized Flask application on an AWS EC2 instance.</i></p>

  ![Docker](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white)
  ![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
  ![Flask](https://img.shields.io/badge/flask-%23000.svg?style=for-the-badge&logo=flask&logoColor=white)
  ![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
</div>

<br />

## 📝 Table of Contents
- [Prerequisites](#-prerequisites)
- [Installation Steps](#-installation-steps)
- [Build and Run the Application](#-build-and-run-the-application)
- [Accessing the Application](#-accessing-the-application)
- [Managing the Container](#-managing-the-container)
- [Cleanup](#-cleanup)
- [Single-Stage vs Multi-Stage Builds](#-single-stage-vs-multi-stage-builds)

---

## 🛠️ Prerequisites

Before you begin, ensure you have the following:

1. ☁️ **Launch an AWS EC2 Instance** (Ubuntu is highly recommended).
2. 🔑 **Connect to your instance** via SSH.
3. 📦 **Update the package list:**
   ```bash
   sudo apt update
   ```

---

## 📥 Installation Steps

### 1️⃣ Clone the Repository
Clone this repository directly to your EC2 instance:
```bash
git clone <repository_url>
```
> **Note:** Replace `<repository_url>` with the actual URL of this repository.

### 2️⃣ Install Docker
Install Docker on your instance (for Ubuntu/Debian):
```bash
sudo apt install docker.io -y
```

### 3️⃣ Setup Docker User Permissions
Add your current user to the `docker` group to run Docker commands without needing `sudo`:
```bash
sudo usermod -aG docker $USER
```
> ⚠️ **Very Important:** You may need to log out and log back in (or run `newgrp docker`) for the permission changes to take effect!

### 4️⃣ Navigate to the App Directory
Change your active directory to the cloned repository and then into the application folder:
```bash
cd <repository_name>/flask-app-dockerized
```
> **Note:** Replace `<repository_name>` with the actual name of the cloned repository folder.


### 5️⃣ Prepare the Dockerfile
You have two options for your Docker build. Copy the code from either the single-stage or multi-stage text file to create your `Dockerfile`:

**🔸 For a Single-Stage Build:**
```bash
cp single-stage.txt Dockerfile
```

**🔹 For a Multi-Stage Build (🌟 Recommended for smaller image sizes):**
```bash
cp multistage.txt Dockerfile
```

---

## 🏗️ Build and Run the Application

### 1️⃣ Build the Docker Image
Build the Docker image using the `Dockerfile` you just set up:
```bash
docker build -t flask-app:latest .
```

### 2️⃣ Verify the Image
Check to ensure your Docker image was created successfully:
```bash
docker images
```

### 3️⃣ Run the Docker Container
Run the container in detached mode (`-d`) and map port `5000` to your host machine:
```bash
docker run -d -p 5000:5000 flask-app:latest
```

### 4️⃣ Verify the Container is Running
Check the status of your running container to make sure everything is okay:
```bash
docker ps
```

---

## 🌐 Accessing the Application

1. 🛡️ **Configure AWS Security Group:** 
   - Go to your **AWS EC2 Console**.
   - Select your running instance.
   - Edit the attached **Security Group**.
   - Add an **Inbound Rule** to allow `Custom TCP` traffic on port `5000` from anywhere (`0.0.0.0/0`).
2. 🌍 **Open in Browser:** 
   - Access the application by navigating to your instance's public IP address followed by the port number:
   ```text
   http://<EC2_PUBLIC_IP>:5000
   ```

---

## ⚙️ Managing the Container

### 🖥️ Accessing the Container Shell
If you ever need to execute commands *inside* the running container:
1. Find the container ID using `docker ps`.
2. Access the bash shell:
   ```bash
   docker exec -it <container_ID> bash
   ```
3. To exit the container shell, simply type:
   ```bash
   exit
   ```

---

## 🧹 Cleanup
To clean up your Docker environment, you can run the following commands sequentially (**Use with caution!**):

1. **🛑 Stop all running containers:**
   ```bash
   docker stop $(docker ps -aq)
   ```

2. **🗑️ Remove all containers (even stopped ones):**
   ```bash
   docker rm -f $(docker ps -aq)
   ```

3. **💥 Delete all images:**
   ```bash
   docker rmi -f $(docker images -q)
   ```

---

## ⚖️ Single-Stage vs Multi-Stage Builds

If you are new to Docker, here is a quick summary of the two build types provided in this repository:

**🔸 Single-Stage Build (`single-stage.txt`)**
- Useful for local development and simple testing.
- It includes all the development dependencies and build tools, making the final image size **larger**.
- Faster to build simply and easier to debug, but not ideal for production environments due to potential security vulnerabilities and large storage demands.

**🔹 Multi-Stage Build (`multistage.txt`)**
- Perfect for **production environments**.
- Uses an initial builder stage to compile code and install dependencies, and then a clean, lightweight runner stage.
- Only the essential runtime components are copied into the final image.
- Results in a significantly **smaller and more secure** Docker image!

<div align="center">
  <br />
  <p><i>Made with ❤️ for Dockerized Deployments!</i></p>
</div>