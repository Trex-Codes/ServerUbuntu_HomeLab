# 🧠 Ubuntu Server HomeLab | Tailscale + Netdata + HDD

A personal server setup on **Ubuntu Server 20.04 LTS** that includes **Tailscale** for secure remote access, **Netdata** for real-time system monitoring, and a **physical external hard drive** for expanded local storage.

---

## 📚 Table of Contents

* [1. Tailscale 🔐](#1-tailscale-)
  * [1.1 Installation 🛠️](#11-installation)
  * [1.2 Configuration ⚙️](#12-configuration)
  * [1.3 Usage 🧪](#13-usage)
* [2. Netdata 📊](#2-netdata-)
  * [2.1 Installation 🛠️](#21-installation)
  * [2.2 Configuration ⚙️](#22-configuration)
  * [2.3 Usage 🧪](#23-usage)
* [3. Disk HDD 💾](#3-disk-hdd-)
  * [3.1 Installation 🛠️](#31-installation)
  * [3.2 Configuration ⚙️](#32-configuration)
  * [3.3 Usage 🧪](#33-usage)
* [Directory Structure 🏗️](#️-directory-structure)


---




## Installation Ubuntu Server 20.04 LTS 🔧

### Prerequisites or Server Setup 🖥️

- Download Ubuntu Server ISO: [ubuntu.com/download/server](https://ubuntu.com/download/server)
- Deploy it on VirtualBox, Proxmox, VMware, or bare metal.
- Configure a **static IP** for LAN access.
- Ensure internet access and SSH are working.

---




## 🧠 General Concepts

This homelab project is built on **Ubuntu Server 20.04 LTS** and is designed to be secure, efficient, and easy to monitor. It includes:

- 🔐 **Tailscale** – Secure remote access to the server via a zero-config VPN.
- 📊 **Netdata** – Real-time system monitoring with a sleek web-based dashboard.
- 💾 **External HDD** – Additional storage space for files, backups, or media.

Tailscale allows encrypted access to your server from anywhere in the world, without needing to expose ports or configure firewalls.  
Netdata provides deep visibility into system metrics such as CPU, RAM, disk I/O, and network traffic, in real-time, and with almost zero configuration.





## 1. Tailscale 🔐

Tailscale is a modern, zero-configuration VPN built on WireGuard. It enables secure access to your private network from anywhere in the world. It’s especially useful for home labs, as it eliminates the need to expose ports or configure complex firewall rules.

### 1.1 Installation 🛠️

#### 🧰 Requirements

- Internet connection
- A [Tailscale account](https://tailscale.com/) (can log in with Google, GitHub, Microsoft, etc.)

---

#### ✅ Installation of Tailscale into the server

Run the official install script:

```bash
curl -fsSL https://tailscale.com/install.sh | sh

# Set Tailscale to Start Automatically
sudo systemctl enable --now tailscaled

```

After installation, start Tailscale and authenticate the server:

```bash
sudo tailscale up
```

#### 🔎 Verifying the Tailscale Connection

To check if your server is successfully connected to your Tailnet:

```bash
tailscale status
```
- This will show:
- Your server's Tailscale IP
- Connection status
- Peers (other devices in the Tailnet)


```bash
100.121.66.123  servertrexcodes  linux   active  direct
```

### 1.2 Configuration ⚙️

You typically won’t need much configuration. However, you can define ACLs or enable subnet routing if needed. Example:

```bash
sudo tailscale up --advertise-tags=tag:server
```

### 1.3 Usage 🧪
You can SSH into the server using the Tailscale IP:

```bash
ssh user@100.121.66.123
```





## 2. Netdata 📊
Netdata is a highly optimized, real-time monitoring tool that provides insightful metrics for CPU, memory, disk I/O, network, and more — all presented via a powerful web-based dashboard.

It’s ideal for keeping track of your server’s health and performance, with minimal resource usage.

### 2.1 Installation 🛠️
#### 🧰 Requirements
- A running Ubuntu 20.04 server (with internet access)
- Root privileges or a sudo-enabled user

#### ✅ Installing Netdata
The easiest way is using the one-line automatic installer provided by Netdata:

```bash
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
```
💡 This will install Netdata, set it up as a systemd service, and start it immediately.

#### 🔎 Verifying the Service
Check the service status:

```bash
sudo systemctl status netdata
```

And verify that Netdata is listening on port `19999`:
```bash
sudo ss -tulpn | grep 19999
```

### 2.2 Configuration ⚙️
The default configuration works out of the box, but you can adjust settings if needed.

Netdata's main config file can be found at:
```bash
/etc/netdata/netdata.conf

# To edit it
sudo nano /etc/netdata/netdata.conf

# Then to restart services
sudo systemctl restart netdata
```

### 2.3 Usage 🧪
To access the Netdata dashboard, open your browser and visit:
```bash
http://<server-ip>:19999
```

If you’re connected via Tailscale, use the Tailscale IP instead:
```bash
http://100.121.66.123:19999
```




