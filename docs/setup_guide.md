# Raspberry Pi Fleet - Setup Guide

## Prerequisites
Before setting up monitoring and automation, ensure you have:
- A fleet of Raspberry Pi devices running [PiSignage OS](https://github.com/colloqi/piSignage)
- A control machine running **Ubuntu Server**, responsible for monitoring the Raspberry Pi devices and managing automation via Ansible
- Prometheus, Grafana, and Ansible installed on the **Ubuntu Server** (control machine)
- Node Exporter installed on each **Raspberry Pi** (Pi 3 to Pi 5 models)

## Installation Steps
1. **Set Up Ansible for Automated Updates**
   - Follow [ansible_setup.md](ansible_setup.md) to configure Ansible for managing updates across Raspberry Pi devices.

2. **Install and Configure Prometheus**
   - Follow [prometheus.md](prometheus.md) for setting up Prometheus as the time-series database to collect monitoring data.

3. **Install and Configure Node Exporter**
   - Follow [node_exporter.md](node_exporter.md) to install and configure Node Exporter on each Raspberry Pi.

4. **Set Up Grafana for Visualization**
   - Follow [grafana.md](grafana.md) to install Grafana, connect it to Prometheus, and import dashboards.

5. **Configure Alerting for Issues**
   - Follow [alerting.md](alerting.md) to configure alerts for device failures, high resource usage, or other anomalies.

## Network Considerations
- All devices communicate using **hostnames**, not static IP addresses.
- Ensure all Raspberry Pi devices are **on the same network** as the control machine running Ubuntu Server.
- Open necessary ports for communication:
  - **9100** (Node Exporter metrics)
  - **9090** (Prometheus web UI)
  - **3000** (Grafana web UI)

## Testing the Setup
Once all components are installed:
1. **Verify Node Exporter is running on each Pi**:
   ```bash
   curl http://<pi-hostname>:9100/metrics
   ```
   - You should see system metrics output.

2. **Check Prometheus Targets**:
   - Open Prometheus at: `http://<monitoring-server-hostname>:9090/targets`
   - Ensure all Raspberry Pi devices are listed and marked as **UP**.

3. **Confirm Grafana Dashboards**:
   - Open Grafana at: `http://<monitoring-server-hostname>:3000`
   - Load a dashboard and check if data is flowing in.

## Security & Hardening
To protect the Raspberry Pi devices and the control machine, implement firewall rules and intrusion prevention measures.

### Dedicated Sudo User for Services
A dedicated hidden user (`pi-admin`) is created to securely run all monitoring and automation services without exposing administrative access to default accounts.
- **Purpose:** Runs Prometheus, Node Exporter, Grafana, and automation tasks.
- **Permissions:** Full `sudo` access, but not used for regular logins.

- **UFW (Uncomplicated Firewall)** is used to restrict access to essential services.
- **Fail2Ban** helps prevent brute-force attacks on SSH and other critical services.

For detailed security configuration, refer to [security.md](docs/security).
