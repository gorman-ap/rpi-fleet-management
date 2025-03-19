# Raspberry Pi Fleet Management

## Project Overview
This repository tracks the **automation, monitoring, and alerting** of Raspberry Pi devices across the company. The goal is to ensure:

- **Automated updates** for OS and software
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
git clone https://github.com/yourusername/raspberry-pi-fleet.git
cd raspberry-pi-fleet
```

### Setup & Installation
See the **docs/setup_guide.md** for detailed setup instructions.

### Running the Monitoring Stack
For detailed instructions on monitoring setup, see **[docs/monitoring.md](docs/monitoring.md)**.

### Automate Updates
Updates are managed using **Ansible** to ensure consistency across all Raspberry Pi devices.

See **docs/ansible_setup.md** for installation and usage instructions.

### Set Up Alerts
See **docs/alerting.md** for alerting configuration.

## How to Contribute
- Submit **Issues** for bugs, feature requests, or improvements.
- Use **Pull Requests (PRs)** to propose changes to scripts or configurations.
- See the **docs/setup_guide.md** for setup instructions.

## Changelog
See [CHANGELOG.md](CHANGELOG.md) for project updates.
