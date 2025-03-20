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

### **Step 1: Set the Hostname**
```bash
sudo hostnamectl set-hostname pibrain
```
This ensures the server is correctly identified on the network.

### **Step 2: Configure SSH Key Authentication**
To enable passwordless SSH authentication for Ansible, generate an SSH key on PiBrain:
```bash
ssh-keygen -t ed25519 -C "ansible@pibrain"
```
Press **Enter** to accept the default location (`~/.ssh/id_ed25519`).

Then, copy the key to each Raspberry Pi:
```bash
ssh-copy-id pi-admin@[device_hostname]
```
Test the connection to ensure SSH key authentication works:
```bash
ssh pi-admin@device-1
```
If successful, Ansible can now connect without requiring a password.
The server will communicate with all Raspberry Pi devices using **hostnames**.

---

## ** Installing & Configuring Monitoring & Automation**

### **Step 1: Install and Configure Prometheus**
Follow [docs/prometheus.md](docs/prometheus.md) to set up Prometheus on the server.

### **Step 2: Install and Configure Grafana**
Follow [docs/grafana.md](docs/grafana.md) to install Grafana and configure dashboards.

### **Step 3: Set Up Ansible for Automation**
Follow [docs/ansible_setup.md](docs/ansible_setup.md) to configure Ansible for managing Raspberry Pi updates.

---

## ** Security & Hardening**

### **Secure Sudo Permissions for Ansible**

To allow Ansible to automate updates and system management securely, configure `pi-admin` with restricted `NOPASSWD` permissions.

Edit the sudoers file:
```bash
sudo visudo
```
Modify the sudoers file to ensure only necessary commands can run without a password, reducing the risk of unintended privilege escalation.
```
pi-admin ALL=(ALL) NOPASSWD: /usr/bin/apt update, /usr/bin/apt upgrade, /usr/bin/apt install, /usr/bin/systemctl restart *, /usr/bin/systemctl enable *, /bin/cp, /bin/mv
```
---

## **Why This Approach?**
Instead of granting full `NOPASSWD: ALL` access, we limit `pi-admin` to only essential commands:
- **System updates (`apt update && apt upgrade`)**: Ensures Ansible can keep the system up to date.
- **Package installations (`apt install`)**: Allows adding necessary software without manual intervention.
- **Service management (`systemctl restart` and `systemctl enable`)**: Ensures Ansible can restart and enable services but **not disable or stop them**.
- **File management (`cp` and `mv`)**: Allows modifying configuration files safely.

 **What This Prevents:**
 `apt remove` or `apt purge` (prevents accidental package deletions)
 `systemctl stop` or `systemctl disable` (prevents critical services from being turned off)
 Full root access (`NOPASSWD: ALL`), ensuring security best practices.

 **This allows:**
- Running system updates (`apt update && apt upgrade`)
- Installing packages (`apt install` but **not** removing them)
- Restarting and enabling services (`systemctl restart` and `systemctl enable`)
- Copying configuration files to system directories (`cp` and `mv`)
- **Does NOT allow full root access (`NOPASSWD: ALL`), ensuring security.**
To secure the server, implement firewall rules and intrusion prevention measures.

### **Firewall & Intrusion Prevention**
- **UFW (Uncomplicated Firewall)** is used to restrict access to essential services.
- **Fail2Ban** helps prevent brute-force attacks on SSH and other critical services.

For detailed security configuration, refer to the [security folder](https://github.com/gorman-ap/rpi-fleet-management/tree/main/docs/security).

---

This guide ensures that your **server (PiBrain) is properly configured, updated, monitored, and secured.** 🚀
