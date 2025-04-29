# Raspberry Pi Fleet Management via Ansible

## Overview

This project automates both the provisioning and maintenance of Raspberry Pi devices using Ansible, operated from a central server (pibrain).
It ensures production signage devices are consistently configured, secured, monitored, and updated — with minimal disruption across multiple locations.

## Prerequisites

Required Packages on PiBrain (Ansible Control Node)
```bash
sudo apt update && sudo apt install -y ansible sshpass
```

## SSH Key Setup
 - Generate an SSH key if you don't have one:
```bash
sh-keygen -t rsa -b 4096 -C "ansible@pibrain"
```
 - Copy the SSH key to the default pi user on the Raspberry Pi:
```bash
ssh-copy-id pi@<hostname>
```
---
### The PiSignages OS installer uses pi as the default user, pi-admin is added to the system through the [provision_new_pi.yaml](https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/config/provision_new_pi.yaml) playbook. As such, the initial provisioning must connect as pi, not pi-admin.
---
## Inventory Structure
 The inventory is organized in ~/ansible/inventory/hosts and divided into groups:
```ini
[group1]
<hostname> ansible_host=<hostname> ansible_user=pi-admin

[group2]
<hostname> ansible_host=<hostname> ansible_user=pi-admin

[group3]
<hostname> ansible_host=<hostname> ansible_user=pi-admin

[server]
pibrain

[raspberry_pis:children]
group1
group2
group3
```

 - Production devices are split across three groups to stagger updates and reduce risk.

 - pibrain (the server) is managed separately.

---
---

# Playbooks
### [provision_new_pi.yaml](https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/config/provision_new_pi.yaml)

   This playbook is used after the initial boot of the new device to automate services/programs and configurations.
- This playbook will:
  - Create a new pi-admin sudo user
  - Copy SSH keys to pi-admin
  - Set a strong password for pi-admin
  - Install critical tools (ufw, fail2ban)
  - Deploy Node Exporter for Prometheus monitoring
  - Apply UFW firewall rules (22, 80, 443, 9100)
  - Enable services on boot
  - Reboot the Pi after provisioning

## Run this manually after device boot:

```bash
ansible-playbook -i ~/ansible/inventory/hosts ~/ansible/configs/provision_new_pi.yaml --limit=IT---Tester -e ansible_user=pi --ask-become-pass
```
(ansible_user=pi is used for the first run only — later management uses pi-admin.)

---

### [maintenance.yaml](https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/config/maintenance.yaml)

This playbook keeps the Pi fleet healthy by automating both apt update & upgrade and restarting the devices

## You can manually update a group like this:

```bash
ansible-playbook -i ~/ansible/inventory/hosts ~/ansible/configs/update_raspberry_pis.yaml --limit group1
```

---
---

# Automation via Cron Jobs
To automate updates without impacting the entire fleet at once:

- Group 1 updates: 8th of the month @ 2:00 AM
- Group 2 updates: 15th of the month @ 2:00 AM
- Group 3 updates: 22nd of the month @ 2:00 AM

Example Crontab Entries (on PiBrain):
```cron
0 2 8 * * ansible-playbook -i ~/ansible/inventory/hosts ~/ansible/configs/update_raspberry_pis.yaml --limit group1
0 2 15 * * ansible-playbook -i ~/ansible/inventory/hosts ~/ansible/configs/update_raspberry_pis.yaml --limit group2
0 2 22 * * ansible-playbook -i ~/ansible/inventory/hosts ~/ansible/configs/update_raspberry_pis.yaml --limit group3
```

---
---

# Why This System Was Built:
This project was designed to create a reliable and lightweight management system for Raspberry Pi signage devices operating across multiple buildings and locations. Simplicity and reliability were prioritized above all else, ensuring minimal disruptions to production while still maintaining security and monitoring standards.

Rather than relying on heavy centralized services or complex monitoring stacks, this system uses Ansible to handle targeted updates, upgrades, and device provisioning automatically. Devices are grouped strategically to stagger updates throughout the month, reducing the risk of widespread outages caused by unexpected package failures.

Prometheus and Grafana are deployed to monitor device availability and system health at a glance, while lightweight tools like UFW and Fail2Ban are configured to improve security without consuming significant resources.

The focus of this setup is on maintainability, speed of deployment, and minimizing the operational footprint — ensuring that production signage devices remain stable, secure, and easy to manage without overcomplicating the infrastructure.
