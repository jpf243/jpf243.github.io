# Docker & Docker Compose Deployment of Uptime Kuma
**Author:** Joaquin Prego  
**GitHub Pages:** https://jpf243.github.io  
**GitHub Username:** jpf243  

## 1. Introduction
The purpose of this project is to install Docker and Docker Compose on a Debian-based virtual machine and deploy an application using containers.  
For this assignment, I chose **Uptime Kuma**, an open-source monitoring tool that provides uptime and service checks through a clean web interface.

This documentation provides all steps required to reproduce the installation so another student can follow it successfully.

## 2. Environment Setup
- **Hypervisor:** VMware Workstation / VMware Player  
- **Virtual Machine OS:** Debian (fresh installation)  
- **User:** Standard user with sudo privileges  
- **Network configuration:** NAT or Bridged (both compatible)  

## 3. Updating Debian
Before installing Docker, update your system:

```bash
sudo apt update
sudo apt upgrade -y
```

## 4. Installing Docker on Debian

### 4.1 Install required packages
```bash
sudo apt install ca-certificates curl gnupg lsb-release -y
```

### 4.2 Add Docker’s official GPG key
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### 4.3 Add Docker’s official repository
```bash
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian   $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 4.4 Update package lists and install Docker
```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

### 4.5 Test Docker installation
```bash
sudo docker run hello-world
```

## 5. Deploying Uptime Kuma using Docker Compose

### 5.1 Create a project directory
```bash
mkdir ~/uptime-kuma
cd ~/uptime-kuma
```

### 5.2 Create a docker-compose.yml file
```bash
nano docker-compose.yml
```

Paste:

```yaml
version: '3'

services:
  uptime-kuma:
    image: louislam/uptime-kuma:latest
    container_name: uptime-kuma
    ports:
      - "3001:3001"
    volumes:
      - ./data:/app/data
    restart: unless-stopped
```

### 5.3 Start the service
```bash
sudo docker compose up -d
```

### 5.4 Confirm the container is running
```bash
sudo docker ps
```

## 6. Accessing Uptime Kuma
Find your VM’s IP:

```bash
ip a
```

Open Uptime Kuma in your browser:

```
http://<VM-IP>:3001
```

Example:

```
http://192.168.202.128:3001
```

You will be prompted to create an admin account.

## 7. File Structure
After deployment, your project folder should look like:

```
uptime-kuma/
 ├─ docker-compose.yml
 └─ data/
```

## 8. Useful Commands

**Stop the container:**
```bash
sudo docker compose down
```

**Start it again:**
```bash
sudo docker compose up -d
```

**View logs:**
```bash
sudo docker logs uptime-kuma
```

## 9. GitHub Pages Submission
1. Create a repository named: `jpf243.github.io`
2. Add a README.md or a dedicated Markdown page containing this documentation.
3. Commit and push.
4. Your GitHub Pages site will be available at:  
   **https://jpf243.github.io**

## 10. Conclusion
This project demonstrates how to install Docker and Docker Compose on a Debian VM and deploy a real application using containerization. Uptime Kuma runs reliably using minimal configuration and serves as an excellent example of service deployment with Docker.

The steps provided here can be replicated to deploy many other open-source services using the same workflow.
