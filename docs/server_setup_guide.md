# Server Setup (PiBrain)

## Prerequisites
Before starting the setup, ensure you have the following hardware:
- A **server or PC** to act as the monitoring and automation control machine (e.g., Raspberry Pi, Ubuntu Server, or other Linux-based system)
- A **power supply** compatible with your hardware
- A **keyboard and monitor** (optional, for initial setup)
- **Ethernet cable or Wi-Fi connection** for networking

---

## ** Installing Ubuntu Server**

### **Step 1: Download Ubuntu Server**
Download the latest version of Ubuntu Server from the official [Ubuntu website](https://ubuntu.com/download/server).

### **Step 2: Create a Bootable USB Drive**
Use **Balena Etcher** to create a bootable USB.

1. Insert a USB flash drive (at least 8GB).
2. Open **Balena Etcher** and select the downloaded Ubuntu Server ISO.
3. Choose the USB drive and click "Write."

### **Step 3: Install Ubuntu Server**
1. Insert the bootable USB into the server.
2. Power on the server and boot from the USB drive.
3. Follow the on-screen instructions to install Ubuntu Server.
4. Set the hostname to **PiBrain** during installation.
5. Create an **admin user** and set up SSH access.

### **Step 4: Update the System**
After installation, update system packages:
```bash
sudo apt update && sudo apt upgrade -y
```
### **Step 5: Dedicated Sudo User for Services**
A dedicated hidden user (`pi-admin`) is created to securely run all monitoring and automation services. For detailed setup instructions, see [docs/security/hidden_sudo_user.md](docs/security/hidden_sudo_user.md).

---

## ** Network & Hostname Setup**
The server will communicate with all Raspberry Pi devices using **hostnames**.

### **Step 1: Set the Hostname**
```bash
sudo hostnamectl set-hostname pibrain
```


---

## ** Installing & Configuring Monitoring & Automation**

### **Step 1: Install and Configure Prometheus**
Follow [docs/prometheus.md](docs/prometheus.md) to set up Prometheus on the server.

### **Step 2: Install and Configure Grafana**
Follow [docs/grafana.md](docs/grafana.md) to install Grafana and configure dashboards.

### **Step 3: Set Up Ansible for Automation**
Follow [docs/ansible_setup.md](docs/ansible_setup.md) to configure Ansible for managing Raspberry Pi updates.

---

---

## ** Security & Hardening**
To secure the server, implement firewall rules and intrusion prevention measures.

### **Firewall & Intrusion Prevention**
- **UFW (Uncomplicated Firewall)** is used to restrict access to essential services.
- **Fail2Ban** helps prevent brute-force attacks on SSH and other critical services.

For detailed security configuration, refer to the [security folder](https://github.com/gorman-ap/rpi-fleet-management/tree/main/docs/security).

---

This guide ensures that your **server (PiBrain) is properly configured, updated, monitored, and secured.**
