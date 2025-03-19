# Grafana Setup Guide

## Overview
This guide provides step-by-step instructions for setting up **Grafana** on the Ubuntu Server. Grafana is used for visualizing system metrics collected by **Prometheus** from Raspberry Pi devices.

---

##  Prerequisites
Before proceeding, ensure you have:
- A server running **Ubuntu Server**.
- **Prometheus installed and running** (see [prometheus.md](prometheus.md)).
- **Firewall configured to allow port 3000** (Grafana UI).

---

##  1. Install Grafana
1. **Add the official Grafana repository:**
   ```bash
   sudo apt update
   sudo apt install -y software-properties-common
   sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
   ```
2. **Install Grafana:**
   ```bash
   sudo apt update && sudo apt install -y grafana
   ```

---

##  2. Enable & Start Grafana Service
1. **Start the Grafana service:**
   ```bash
   sudo systemctl start grafana-server
   ```
2. **Enable Grafana to start on boot:**
   ```bash
   sudo systemctl enable grafana-server
   ```
3. **Verify that Grafana is running:**
   ```bash
   systemctl status grafana-server
   ```

---

##  3. Access Grafana Web Interface
1. Open a web browser and go to:
   ```
   http://<server-hostname>:3000
   ```
2. **Login with default credentials:**
   - Username: `admin`
   - Password: `admin`
3. **Change the default password** when prompted.

---

##  4. Configure Prometheus as a Data Source
1. **Go to**: **Grafana > Settings > Data Sources**.
2. Click **Add Data Source**.
3. Select **Prometheus**.
4. Set the URL to:
   ```
   http://<server-hostname>:9090
   ```
5. Click **Save & Test**.

---

##  5. (Optional) Import Grafana Dashboards
1. **Go to**: **Grafana > Dashboards > Import**.
2. **Upload the pre-configured dashboard file:**
   ```bash
   sudo wget -O /etc/grafana/provisioning/dashboards/default.json \
   https://raw.githubusercontent.com/yourrepo/raspberry-pi-monitoring/main/configs/grafana_dashboard.json
   ```
3. In Grafana, select **Prometheus** as the data source.
4. Click **Import**.

---

##  Grafana is Now Installed & Configured!
At this point, **Grafana is actively visualizing Prometheus metrics** from Raspberry Pi devices. To access your dashboards:
```
http://<server-hostname>:3000
```
For more customizations, explore Grafana’s dashboard editor and alerting features.

