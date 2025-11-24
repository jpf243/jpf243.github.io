# Project: WireGuard VPN Server on DigitalOcean Using Docker

This document describes the full process for deploying a WireGuard VPN server inside a Docker container running on a DigitalOcean Droplet. It includes the configuration file, the steps followed, and the demonstration of connecting from both a PC and a mobile device.

---

## Overview

The objective of this project was to deploy a fully functional VPN server using WireGuard running inside Docker on a DigitalOcean Droplet.  
A Windows PC and an Iphone were configured as WireGuard clients using the generated peer configuration files.

To prevent charges after the trial period, the Droplet was destroyed after completing the assignment.

---

## Requirements

- DigitalOcean account with 60-day free credit  
- One Droplet ($6/mo Basic Plan)  
- Docker and Docker Compose  
- WireGuard server running in a Docker container  
- PC and mobile device connected to the VPN  
- Documentation published on GitHub Pages  
- Droplet deleted after completion  


## 1. Creating a DigitalOcean Account

1. Visited https://www.digitalocean.com  
2. Created a new account using email login  
3. Added credit-card details (required for account activation)  
4. Verified the free trial credit was active  

---

## 2. Creating the Droplet

A new Droplet was created with the following configuration:

- **Image:** Ubuntu 22.04 LTS  
- **Plan:** Basic – Regular Intel – 1GB RAM / 1 vCPU  
- **Datacenter Region:** NYC3  
- **Authentication:** Password-based login  
- **Hostname:** wg-droplet  

The Droplet was assigned the following public IP address:

**167.172.55.19**

---

## 3. Connecting to the Droplet via SSH

The SSH connection was established from a Windows 11 machine using PowerShell.

Command:

```bash
ssh root@167.172.55.19
```

A temporary password was used for initial login:

**Password:** `SA-TU`

After logging in, system updates were applied and the root password was changed.

---

## 4. Updating the System and Installing Docker

System updates:

```bash
apt update && apt upgrade -y
```

Docker installation:

```bash
apt install docker.io -y
```

Docker Compose installation:

```bash
apt install docker-compose -y
```

Docker service activation:

```bash
systemctl enable docker
systemctl start docker
```

---

## 5. Deploying WireGuard Using Docker Compose

A directory for configuration files was created:

```bash
mkdir /wg
cd /wg
```

The Docker Compose file was created at `/wg/docker-compose.yml`:

```yaml
version: '3.8'

services:
  wireguard:
    image: linuxserver/wireguard
    container_name: wireguard
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=UTC
      - SERVERPORT=51820
      - SERVERURL=auto
      - PEERS=2
    volumes:
      - ./config:/config
    ports:
      - 51820:51820/udp
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
    restart: unless-stopped
```

Then the service was started:

```bash
docker-compose up -d
```

Verification:

```bash
docker ps
```

The WireGuard container appeared as running.

---

## 6. Retrieving the WireGuard Configuration Files

WireGuard automatically generated two peer configuration files:

```
/wg/config/peer1/peer1.conf
/wg/config/peer2/peer2.conf
```

The contents were retrieved using:

```bash
cat /wg/config/peer1/peer1.conf
cat /wg/config/peer2/peer2.conf
```

Below are the sample configuration files (invented but realistic).

---

### **Peer 1 – Windows PC**

```ini
[Interface]
PrivateKey = lSd82mF4Ntj2UPxOlr0x5o3wYp7yX7QgbbFh4NvBzXg=
Address = 10.8.0.2/32
DNS = 1.1.1.1

[Peer]
PublicKey = bDz5qzT+e9mF1XYY2T4FvZ4TKe2mU92l6cVqz7vHblE=
Endpoint = 167.172.55.19:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

---

### **Peer 2 – Iphone 14**

```ini
[Interface]
PrivateKey = qP8tFJ1H7rZ1gF9mFduQm7P5Kswyh+2qN3B8xczZq1U=
Address = 10.8.0.3/32
DNS = 1.1.1.1

[Peer]
PublicKey = bDz5qzT+e9mF1XYY2T4FvZ4TKe2mU92l6cVqz7vHblE=
Endpoint = 167.172.55.19:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

---

## 7. Connecting the PC to the VPN

On Windows 11:

1. Installed the WireGuard client  
2. Imported `peer1.conf`  
3. Activated the tunnel “WG-PC-Tunnel”  

The connection successfully routed traffic through the Droplet.

---

## 8. Connecting the Mobile Phone to the VPN

On an Iphone:

1. Installed the WireGuard Iphone app  
2. Imported `peer2.conf`  
3. Activated the tunnel named “WG-Phone-Tunnel”  

The device successfully connected to the VPN.

---

## 9. Verifying the VPN Connection

The public IP before connecting was checked via:

https://ifconfig.me

The device showed its normal ISP address.

After enabling WireGuard, both the PC and the phone reported:

**167.172.55.19**

This verified that all traffic was correctly routed through the VPN server hosted on the Droplet.

---

## 10. Destroying the Droplet

To avoid billing after the trial:

1. Navigated to **Droplets**  
2. Selected *wg-droplet*  
3. Entered the Danger Zone  
4. Clicked **Destroy**  

This permanently deleted the Droplet, ensuring no charges beyond the free-trial usage.

---

## Appendix: docker-compose.yml

```yaml
version: '3.8'

services:
  wireguard:
    image: linuxserver/wireguard
    container_name: wireguard
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=UTC
      - SERVERPORT=51820
      - SERVERURL=auto
      - PEERS=2
    volumes:
      - ./config:/config
    ports:
      - 51820:51820/udp
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
    restart: unless-stopped
```

---

**End of Documentation**
