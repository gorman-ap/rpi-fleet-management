# Node Exporter Setup Guide

## 1. Install Node Exporter

### **For 32-bit Raspberry Pi (ARMv7)**
1. Download the latest Node Exporter binary:
   ```bash
   wget https://github.com/prometheus/node_exporter/releases/download/v1.9.0/node_exporter-1.9.0.linux-armv7.tar.gz
   ```
2. Extract the file:
   ```bash
   tar -xvzf node_exporter-1.9.0.linux-armv7.tar.gz
   ```
3. Move the binary to `/usr/local/bin/` for system-wide use:
   ```bash
   sudo mv node_exporter-1.9.0.linux-armv7/node_exporter /usr/local/bin/
   ```

### **For 64-bit Raspberry Pi (ARM64)**
1. Download the latest Node Exporter binary:
   ```bash
   wget https://github.com/prometheus/node_exporter/releases/download/v1.9.0/node_exporter-1.9.0.linux-arm64.tar.gz
   ```
2. Extract the file:
   ```bash
   tar -xvzf node_exporter-1.9.0.linux-arm64.tar.gz
   ```
3. Move the binary to `/usr/local/bin/` for system-wide use:
   ```bash
   sudo mv node_exporter-1.9.0.linux-arm64/node_exporter /usr/local/bin/
   ```

---

## 2. Create a Dedicated User for Node Exporter
To improve security and prevent running services as root, we create a dedicated user.

1. Create the user:
   ```bash
   sudo useradd -m -s /bin/bash pi-admin
   ```
2. Set a password for `pi-admin`:
   ```bash
   sudo passwd pi-admin
   ```
3. Give `pi-admin` sudo privileges:
   ```bash
   sudo usermod -aG sudo pi-admin
   ```

---

## 3. Configure Node Exporter as a Systemd Service

1. Download the pre-configured service file:
   ```bash
   sudo wget -O /etc/systemd/system/node_exporter.service \
   https://github.com/gorman-ap/rpi-fleet-management/blob/main/docs/config/node_exporter.service
   ```
2. Move the service file to the correct location:
   ```bash
   sudo mv ~/node_exporter.service /etc/systemd/system/
   ```
3. Set ownership of Node Exporter:
   ```bash
   sudo chown pi-admin:pi-admin /usr/local/bin/node_exporter
   ```
4. Reload systemd and enable the service:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable node_exporter
   sudo systemctl start node_exporter
   ```
5. Verify that Node Exporter is running:
   ```bash
   systemctl status node_exporter
   curl http://localhost:9100/metrics
   ```

---

## 4. Firewall Configuration (If UFW is Enabled)
If UFW (Uncomplicated Firewall) is running, ensure port `9100` is allowed on each Raspberry Pi:
   ```bash
   sudo ufw allow 9100/tcp
   sudo ufw reload
   ```

---

## ✅ Node Exporter is Now Installed and Running!
At this point, **Node Exporter is actively collecting system metrics** and should be accessible via Prometheus.

To verify, navigate to:
```
http://<pi-hostname>:9100/metrics
```
If you see output, Node Exporter is working correctly!
