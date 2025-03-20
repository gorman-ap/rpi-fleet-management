## **📝 Changelog – March 20, 2025**

### **🔧 Infrastructure & Configuration Updates**
- **Ansible Inventory Moved**:  
  - Relocated inventory file from `/etc/ansible/hosts` → `~/ansible/inventory/hosts`  
  - Updated `ansible.cfg` to reflect new inventory path  

- **Ansible SSH Key Authentication Fixes**:  
  - Added `ansible_user=pi-admin` to inventory file  
  - Configured `~/.ansible.cfg` to ensure proper SSH key authentication  
  - Encountered and investigated ansible-playbook hanging at the Gathering Facts step. 
  - Issue persists despite SSH key-based authentication working manually. Further debugging required.  

- **Sudoers Configuration for Ansible Automation**:  
  - Updated `/etc/sudoers` to allow Ansible to run commands without requiring a password:  
    ```bash
    pi-admin ALL=(ALL) NOPASSWD: /usr/bin/apt update, /usr/bin/apt upgrade, /usr/bin/apt install, /usr/bin/systemctl restart *, /usr/bin/systemctl enable *, /bin/cp, /bin/mv
    ```
  - Added section to **Server Setup Guide** explaining why these permissions were necessary  

### **🔒 Security & Fail2Ban Fixes**
- **Fail2Ban Lockout Issue**:  
  - Suspected **self-lockout** while testing Ansible on `BSM---Lunchroom`  
  - Manually **unbanned** IP using:  
    ```bash
    sudo fail2ban-client set sshd unbanip <IP>
    ```
  - Letting ban run its course to test resolution before adjusting settings  

### **📄 Documentation Updates**
- **Updated Server Setup Guide**
  - Moved **Dedicated Sudo User Setup** earlier in the guide  
  - Added **Sudoers Config Section** for Ansible automation  
  - Edited **Network & Hostname Setup** to clarify step order  

- **Updated Ansible Setup Guide**
  - Changed references from `inventory.ini` → `inventory/hosts`  
  - Added missing steps for SSH key setup  
