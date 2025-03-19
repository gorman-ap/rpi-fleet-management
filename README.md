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
