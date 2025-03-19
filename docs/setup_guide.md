# Raspberry Pi Fleet - Setup Guide

## Prerequisites
Before setting up monitoring and automation, ensure you have:
- A fleet of Raspberry Pi devices running [PiSignage OS](https://github.com/colloqi/piSignage)
- A control machine running **Ubuntu Server**, responsible for monitoring the Raspberry Pi devices and managing automation via Ansible
- Prometheus, Grafana, and Ansible installed on the **Ubuntu Server** (control machine)
- Node Exporter installed on each **Raspberry Pi** (Pi 3 to Pi 5 models)

---

## ** Installing PiSignage OS on Raspberry Pi**

### **Step 1: Download PiSignage OS**
Download the latest version of PiSignage OS from the official [GitHub repository](https://github.com/colloqi/piSignage).

### **Step 2: Flash the OS to an SD Card**
Use **Raspberry Pi Imager** or **Balena Etcher** to flash the `.img` file onto a microSD card.

1. Insert your microSD card into your computer.
2. Open **Raspberry Pi Imager** and select "Custom Image."
3. Choose the downloaded PiSignage OS `.img` file.
4. Select your microSD card and click "Write."

### **Step 3: Boot Up the Raspberry Pi**
1. Insert the microSD card into the Raspberry Pi.
2. Power on the device and wait for the OS to initialize.
3. Connect the Raspberry Pi to the network via **Ethernet** or **Wi-Fi**.

### **Step 4: Update the System**
After the first boot, update the system packages:
```bash
sudo apt update && sudo apt upgrade -y
```

---
### **Step 5: Hostname Assignment via PiSignage Portal**
Hostnames for each Raspberry Pi are automatically set through the **PiSignage Portal** when the device is registered. Ensure that all devices are properly added and assigned hostnames in the PiSignage management interface.

## ** Ensure Hostname-Based Communication**
All devices communicate using **hostnames**, with no manual IP assignments. Ensure your network's DNS resolves hostnames correctly for seamless communication between devices
---

## ** Installing & Configuring Monitoring & Automation**

### **Step 1: Install and Configure Node Exporter**
Follow [docs/node_exporter.md](docs/node_exporter.md) to install and configure Node Exporter on each Raspberry Pi.

### **Step 2: Verify Node Exporter is Running on Each Pi**
```bash
curl http://<pi-hostname>:9100/metrics
```
- You should see system metrics output.

### **Step 1: Set Up Ansible for Automated Updates**
Follow [ansible_setup.md](ansible_setup.md) to configure Ansible for managing updates across Raspberry Pi devices.




---

## ** Security & Hardening**
To protect the Raspberry Pi devices and the control machine, implement firewall rules and intrusion prevention measures.

### **Dedicated Sudo User for Services**
A dedicated hidden user (`pi-admin`) is created to securely run all monitoring and automation services. For detailed setup instructions, see [docs/security/hidden_sudo_user.md](docs/security/hidden_sudo_user.md).

### **Firewall & Intrusion Prevention**
- **UFW (Uncomplicated Firewall)** is used to restrict access to essential services.
- **Fail2Ban** helps prevent brute-force attacks on SSH and other critical services.

For detailed security configuration, refer to the [security folder](https://github.com/gorman-ap/rpi-fleet-management/tree/main/docs/security).

---



---

This guide ensures that your **Raspberry Pi fleet is properly configured, updated, monitored, and secured.** 🚀
