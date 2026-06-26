# 🐧 VM 101: Debian 12 - Portainer & Storage Host

This Virtual Machine serves as the main application and central engine of my Homelab. It runs **Docker** to host a variety of services and **Samba** to share files across the local network. It uses **Portainer** as the centralized dashboard to deploy and monitor my containers.

## 📊 Environment Specifications
* **Operating System:** Debian 12 (Bookworm)
* **IP Address:** `192.168.0.11`
* **Management UI:** Portainer Community Edition (Port `9443`)

---

## 📁 Storage & Samba Configuration
The external 250GB drive is mounted on Debian at `/media/hdmedia`. The folders under `/media` are shared via Samba across the local network, acting as my local cloud storage. This same root path is utilized by media-related Docker containers such as Jellyfin, Transmission, and Nextcloud.

### Production Storage Layout
* **Media Assets:** `/media/hdmedia` (Bound to Jellyfin, Transmission, and audio streams)
* **Application Configs:** `/docker` (Centralized persistent volume mapping for internal container state)

---

## 🐋 Stack Management & Backup Strategy

All containers are deployed using **Portainer Stacks** (Docker Compose format) directly via the Portainer Web UI Editor. 

This repository functions as an **Infrastructure-as-Code Backup Registry**. Every time a stack is successfully modified and deployed via Portainer, its corresponding Compose file is version-controlled here to guarantee disaster recovery.

### Backup Registry Layout
```text
docker/            
└── [service-name].yml    <-- Production-ready YAML file backup