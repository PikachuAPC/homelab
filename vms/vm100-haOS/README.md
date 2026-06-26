# 🏠 VM 100: Home Assistant OS (HAOS)

This dedicated Virtual Machine runs **Home Assistant OS (HAOS)**. It serves as the automation hub for home infrastructure and acts as the edge network ingress point for the entire Homelab ecosystem.

## 📊 Environment Specifications
* **Operating System:** Home Assistant OS (Appliance Image)
* **IP Address:** `192.168.0.33`
* **Management UI:** Home Assistant Dashboard (Port `8123`)

---

## 🌐 Edge Networking & Ingress Role

Beyond home automation, this VM plays a critical role in the global network architecture of this lab by hosting the **Cloudflare Tunnel Add-on** (`cloudflared`). This is because haOS is the most isolated VM; I usually test things on the Debian machine, so if I break something, the tunnel is safe.


---

## 💾 Backup & Disaster Recovery
* **Instance Snapshots:** Full VM snapshots managed directly via Proxmox VE schedules.
* **Application Backups:** Daily automated full backups generated natively by HAOS for rapid container state recovery.
* **Physical Copy:** I copy EVERYTHING from the bare metal machine to my PC to ensure I have at least some data to start with again (even tho this is most likely a bad idea as i could just restore from the snapshot lmao).