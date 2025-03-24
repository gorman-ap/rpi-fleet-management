# Raspberry Pi Fleet Management

## Project Overview
This repository tracks the **automation, monitoring, and alerting** of Raspberry Pi devices across the company running [PiSignage OS](https://github.com/colloqi/piSignage) that display production data for workers on the factory floor. The goal is to ensure:

- **Automated updates** for software
- **Centralized monitoring** using Prometheus & Grafana
- **Log collection and alerting** for failures or anomalies

## Key Features
- **Automated system updates** using Ansible playbooks for seamless updates across all Raspberry Pi devices
- **Real-time monitoring dashboards** for CPU, memory, and network usage
- **Alerting system** for device failures and resource spikes

## Custom PiSignage OS Development

In addition to monitoring and automation, this project also includes an experimental effort to create a **custom OS image** for Raspberry Pi devices. The goal is to improve compatibility with newer Raspberry Pi models and tailor the operating system specifically for kiosk-style digital signage. 

### Why Custom?
- **Official PiSignage OS** is currently not compatible with newer Pi hardware (e.g., Raspberry Pi 5).
- By building a custom solution, we can:
  - Ensure compatibility with the latest Raspberry Pi models.
  - Strip down unnecessary services for better performance.
  - Integrate Node Exporter and other monitoring tools out-of-the-box.

### Current Progress
- Base image is built on **Raspberry Pi OS Lite**.
- PiSignage components are being ported manually to function as a service.
- Work is ongoing to enable browser kiosk mode and system monitoring on boot.

This portion of the project is still under development, and updates will be added to the repository once a stable image is available. [Current build](https://github.com/gorman-ap/rpi-fleet-management/blob/main/Fleet%20Base%20Image/0.5.0-pre.md)

## Project Status
- [ ] Implement automated updates using Ansible
- [x] Configure Prometheus scraping for all devices
- [x] Design custom Grafana dashboards
- [ ] Set up alerting system
- [ ] Test and refine monitoring & automation

## Getting Started
### Clone the Repository
```bash
git clone https://github.com/gorman-ap/raspberry-pi-fleet.git
cd raspberry-pi-fleet
```

### Setup & Installation
See [docs/README.md](https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/README.md) for detailed setup instructions.


## How to Contribute
- Submit **Issues** for bugs, feature requests, or improvements.
- Use **Pull Requests (PRs)** to propose changes to scripts or configurations.
- See the **docs/README.md** for setup instructions.

## Changelog
See [CHANGELOG.md](CHANGELOG.md) for project updates.
