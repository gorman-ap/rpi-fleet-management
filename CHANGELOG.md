# Changelog - April 29, 2025

## Ansible Provisioning System Finalization

- Provisioning Playbook Improvements:
  - Full automation of new device setup finalized
  - Playbook now automatically creates the pi-admin user with a pre-set password
  - Authorized SSH keys are copied correctly from initial user (pi) to pi-admin
  - Node Exporter is installed based on Pi architecture (ARMv7 vs ARM64)
  - System services (node_exporter, ufw, fail2ban) are enabled and configured

- Inventory & Host Management:
  - Clarified workflow for initial provisioning:
    - Devices boot with default PiSignageOS hostname
    - ssh-copy-id is performed manually once for the pi user
    - Initial provisioning playbook (provision_new_pi.yaml) is run with temporary -u pi
    - Post-provisioning, all future playbooks connect via pi-admin

- Maintenance Playbook Updates:
  - Update and reboot playbook staggered across groups using cron jobs
  - Groups scheduled to update at 2:00 AM on the 8th, 15th, and 22nd of each month

- Documentation Updates:
  - Major refresh of README.md to reflect real-world production workflow
  - Added explanation for PiSignage OS constraints and hostname behavior
  - Provided clear cron scheduling examples
  - Clarified why the system is intentionally lightweight and minimal


---
---

# Changelog – April 24, 2025

## Custom PiSignage OS Update

- The custom OS project for Raspberry Pi 5 has been officially shelved due to complexity and the decision to keep signage devices lightweight and production-safe

## Ansible Infrastructure Enhancements

- Created a new provision-new-pi playbook to fully set up Raspberry Pi devices upon deployment, including:
  - Adding pi-admin user with sudo privileges
  - Installing and enabling node_exporter, ufw, and fail2ban
  - Configuring UFW rules and auto-enabling services
  - Rebooting after setup

- Playbooks are now split into purpose-driven files (update.yaml, provision-new-pi.yaml)
- Organized inventory groups by device deployment phase (group1, group2, group3) under raspberry_pis
- Future devices now follow hostname-based provisioning logic for smooth replacement and reimaging
- Updated documentation and configs for Ansible

---

---

# Custom OS Development – March 21, 2025

## Initial Documentation
- Added new section to the main README highlighting the experimental **custom PiSignage OS** project.
- Outlined goals for better support on newer hardware (e.g., Raspberry Pi 5) and integration of monitoring tools.

## Versioning
- Logged failed initial test as version [**`v0.5.0`**](https://github.com/gorman-ap/rpi-fleet-management/blob/main/Fleet%20Base%20Image/0.5.0-pre.md) in its own markdown file for tracking builds and issues.
- Custom OS is currently based on **Raspberry Pi OS Lite**, with manual migration of PiSignage components.

---

---

# Changelog – March 20, 2025

## Infrastructure & Configuration Updates
- Ansible Inventory Moved:  
  - Relocated inventory file from `/etc/ansible/hosts` → `~/ansible/inventory/hosts`  
  - Updated `ansible.cfg` to reflect new inventory path  

- Ansible SSH Key Authentication Fixes:  
  - Added `ansible_user=pi-admin` to inventory file  
  - Configured `~/.ansible.cfg` to ensure proper SSH key authentication  
  - Encountered and investigated ansible-playbook hanging at the Gathering Facts step. 
  - Issue persists despite SSH key-based authentication working manually. Further debugging required.  

- Sudoers Configuration for Ansible Automation**:  
  - Updated `/etc/sudoers` to allow Ansible to run commands without requiring a password:  
    ```bash
    pi-admin ALL=(ALL) NOPASSWD: /usr/bin/apt update, /usr/bin/apt upgrade, /usr/bin/apt install, /usr/bin/systemctl restart *, /usr/bin/systemctl enable *, /bin/cp, /bin/mv
    ```
  - Added section to **Server Setup Guide** explaining why these permissions were necessary  

## Security & Fail2Ban Fixes
- Fail2Ban Lockout Issue:  
  - Suspected **self-lockout** while testing Ansible on `BSM---Lunchroom`  
  - Manually **unbanned** IP using:  
    ```bash
    sudo fail2ban-client set sshd unbanip <IP>
    ```
  - Letting ban run its course to test resolution before adjusting settings  

##  Documentation Updates
- Updated Server Setup Guide
  - Moved **Dedicated Sudo User Setup** earlier in the guide  
  - Added **Sudoers Config Section** for Ansible automation  
  - Edited **Network & Hostname Setup** to clarify step order  

- **Updated Ansible Setup Guide**
  - Changed references from `inventory.ini` → `inventory/hosts`  
  - Added missing steps for SSH key setup  
