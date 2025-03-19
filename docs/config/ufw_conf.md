# UFW Configuration for Raspberry Pi Monitoring

## Overview
This configuration is designed to secure the Raspberry Pi devices while allowing necessary traffic for monitoring, automation, and remote management. **All incoming traffic is denied by default except for explicitly allowed services.**

---

## Default Policies
- **Deny all incoming traffic** (unless explicitly allowed)
- **Allow all outgoing traffic**
- **Deny all routed traffic** (prevents forwarding by default)

---

## Allowed Ports & Services
| Port  | Protocol | Purpose |
|-------|----------|--------------------------------------------------|
| 22    | TCP      | SSH access for remote management |
| 9100  | TCP      | Node Exporter (metrics collection) |
| 9090  | TCP      | Prometheus web interface & API |
| 8000  | TCP      | PiSignage OS Web Interface |
| 8001  | TCP      | PiSignage OS Internal Service |
| 3000  | TCP      | Grafana web interface for visualizing metrics |

---

## UFW Rules Configuration

UFW rules are stored persistently in:
- **IPv4 rules**: `/etc/ufw/user.rules`
- **IPv6 rules**: `/etc/ufw/user6.rules`

These files contain all active firewall rules and are modified when you run `ufw allow`, `ufw deny`, or `ufw delete` commands.

To manually view the saved rules:
```bash
sudo cat /etc/ufw/user.rules
```

To apply these firewall rules, run the following commands on each Raspberry Pi and the monitoring server:

```bash
# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw default deny routed

# Allow SSH (22/tcp)
sudo ufw allow 22/tcp

# Allow Prometheus (9090/tcp)
sudo ufw allow 9090/tcp

# Allow Node Exporter (9100/tcp)
sudo ufw allow 9100/tcp

# Allow Grafana (3000/tcp)
sudo ufw allow 3000/tcp

# Allow internal applications (8000, 8001/tcp)
sudo ufw allow 8000/tcp
sudo ufw allow 8001/tcp

# Enable UFW
sudo ufw enable

# Verify UFW Status
sudo ufw status verbose
```

---

## ✅ Firewall Configuration Complete
UFW is now set up to secure your devices while allowing essential monitoring and management services. To adjust firewall rules, use:

```bash
sudo ufw allow <port>/tcp  # Allow a new port
sudo ufw deny <port>/tcp   # Block a port
sudo ufw delete allow <port>/tcp  # Remove a rule
