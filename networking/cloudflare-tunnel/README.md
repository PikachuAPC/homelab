# 🌐 Cloudflare Zero Trust Tunnel Architecture

This section documents the external access layer and routing for my Homelab ecosystem. All inbound traffic is managed through a **Cloudflare Tunnel** linked to my primary domain (`pikachuapc.es`) and secondary zones.

## 🛡️ Infrastructure Overview
* **Security Edge:** Traffic is proxied through Cloudflare's network, which enforces automatic TLS/SSL encryption and hides my residential public IP.
* **Firewall Bypass:** The `cloudflared` daemon runs as a native **Home Assistant Add-on** inside VM 100. It establishes a secure outbound connection to Cloudflare Zero Trust, eliminating the need for port forwarding on my residential router and successfully bypassing Carrier-Grade NAT (CGNAT).
* **Cross-VM Routing:** Since both virtual machines reside on the same Proxmox virtual bridge, the Home Assistant tunnel seamlessly bridges external requests to the microservices running inside the Debian host (VM 101).

---

## 🔀 Ingress Routing Table

Below is the live production mapping configured within the Cloudflare Zero Trust dashboard. It translates external public hostnames into internal private IPs and ports across my Proxmox VE virtual instances:

| # | Public Hostname | Internal Service | Target Endpoint (LAN IP:Port) | Environment Segment |
| :-: | :--- | :--- | :--- | :--- |
| 1 | `ha.yourdomain.com` | Home Assistant | `http://192.168.0.33:8123` | VM 100 (HAOS) |
| 2 | `media.yourdomain.com` | Jellyfin Media Server | `http://192.168.0.11:8096` | VM 101 (Docker Host) |
| 3 | `vault.yourdomain.com` | Password Manager / Vault | `http://192.168.0.11:88` | VM 101 (Docker Host) |
| 4 | `cloud.yourdomain.com` | Nextcloud Private Cloud | `http://192.168.0.11:7800` | VM 101 (Docker Host) |
...

> *Note: Actual public hostnames and domains have been anonymized for security reasons.*

> The list is updated as I add more services to my homelab. It may be that not every service is accessible to the public. 

---

## 🔒 Access & Edge Security Policies
* **Encrypted End-to-End:** All connections are secured using Cloudflare’s Edge TLS certificates.
* **Zero Trust Ingress:** Only traffic matching these explicit hostname rules is routed internally. Any unmapped subdomains or direct IP attempts at the edge are automatically dropped with a `404 Not Found` response.