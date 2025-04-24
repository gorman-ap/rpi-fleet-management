# Ansible Setup

## Overview
This documentation outlines the configuration and automation of Raspberry Pi devices using Ansible from the server [PiBrain](https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/server_setup.md). It focuses on managing system packages, security tools, and service provisioning on production devices deployed across multiple locations.


## Prerequisites
### Required Packages on the server [(PiBrain)](https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/server_setup.md) 
- Ensure the following packages are installed on the Ansible control node:
```bash
sudo apt update && sudo apt install -y ansible sshpass
```

### SSH Key Setup
- Generate an SSH key on the control node if you haven't already:
```bash
ssh-keygen -t rsa -b 4096 -C "ansible@control-node"
```
- Copy the public SSH key to each Raspberry Pi:
```bash
ssh-copy-id pi-admin@<hostname>
```

## Inventory Setup
- Your inventory file is stored inside the Ansible project directory for easier version control and separation from system files. 
```bash
~/ansible/inventory/hosts
```
- Because this is a live production environment the [hosts](https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/config/hosts) file is seperated into three groups to stagger updates. This prevents widespread issues if an update causes problems with the custom PiSignage OS.


## Ansible Playbook Structure
- This repository contains multiple playbooks located in the configs/ folder:
  - [update_raspberry_pis.yaml](https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/config/maintenance.yaml): Used to apply updates, upgrades, and reboot devices in maintenance cycles.
  - [provision_new_pi.yaml](https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/config/provision_new_pi.yaml): Used for provisioning freshly imaged Raspberry Pi devices.


## Running the Playbook
- The [provision_new_pi.yaml](https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/config/provision_new_pi.yaml) is ran manually after the device is booted up for the first time. To do so run:
```bash
ansible-playbook -i ~/ansible/inventory/hosts update_raspberry_pis.yaml
```


## Automating the Maintenance Playbook
- To fully automate and stagger updates, three cron jobs must be created on your control node [(PiBrain)](https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/server_setup.md) to target different device groupt across the month:

|Group |	Cron Schedule (2:00 AM)	| Command|
------|-------------------------|--------|
group1|	8th of the month | ansible-playbook -i ~/ansible/inventory/hosts update_raspberry_pis.yaml --limit group1
group2|	15th of the month	| ansible-playbook -i ~/ansible/inventory/hosts update_raspberry_pis.yaml --limit group2
group3|	22nd of the month	| ansible-playbook -i ~/ansible/inventory/hosts update_raspberry_pis.yaml --limit group3


. Each job will target a different group, each a week apart. You can find the crontab configuration [here](https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/config/crontab%20*).
