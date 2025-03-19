# Fail2Ban Setup for Raspberry Pi Monitoring

## Overview
Fail2Ban helps protect Raspberry Pi devices from brute-force attacks by monitoring failed login attempts and banning IP addresses after multiple failed attempts.

---

## ** Install Fail2Ban**
Fail2Ban is included in most Linux distributions. Install it using:
```bash
sudo apt update && sudo apt install fail2ban -y
```

---

## ** Configure Fail2Ban**

### **Step 1: Create a Local Configuration File**
By default, Fail2Ban uses `/etc/fail2ban/jail.conf`, but we create a custom local file to prevent updates from overwriting our settings:
```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

### **Step 2: Edit the Configuration**
Open the configuration file for editing:
```bash
sudo nano /etc/fail2ban/jail.local
```
Find the `[sshd]` section and update it as follows:
```ini
[sshd]
enabled = true
port = 22
maxretry = 3
bantime = 3600
```
- **enabled = true** → Enables Fail2Ban for SSH.
- **port = 22** → Ensures it applies to SSH.
- **maxretry = 3** → Bans after 3 failed attempts.
- **bantime = 3600** → Bans IPs for 1 hour.

Save and exit: `CTRL + X`, then `Y`, then `Enter`.

---

## ** Restart and Enable Fail2Ban**
Restart the service and enable it on boot:
```bash
sudo systemctl restart fail2ban
sudo systemctl enable fail2ban
```

---

## ** Verify Fail2Ban is Active**
To check if Fail2Ban is running:
```bash
sudo systemctl status fail2ban
```

To see currently banned IPs:
```bash
sudo fail2ban-client status sshd
```

---

##  Fail2Ban Configuration Complete
Fail2Ban is now monitoring SSH for brute-force attacks and will ban repeated failed attempts automatically. 
