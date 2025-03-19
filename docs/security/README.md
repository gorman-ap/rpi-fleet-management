# Security & Hardening Documentation

## Overview
This section contains security configurations and best practices for protecting the **Raspberry Pi Fleet Management System** and its components.

##  Table of Contents
- **[Hidden Sudo User](hidden_sudo_user.md)** → Creating a non-root administrative user for services.
- **[UFW Firewall Rules](ufw_setup.md)** → Configuring Uncomplicated Firewall (UFW) to restrict access.
- **[Fail2Ban Setup](fail2ban_setup.md)** → Protecting against brute-force attacks on SSH.

##  Security Best Practices
- **Disable root login** to prevent unauthorized access.
- **Use SSH keys** instead of password authentication for secure remote access.
- **Restrict services** to only necessary ports using **UFW**.
- **Monitor failed login attempts** with **Fail2Ban** to block repeated offenders.

##  How to Use These Docs
1. **New Setup?** Start with **Hidden Sudo User** to secure administrative access.
2. **Configuring Firewall?** Follow the **UFW Firewall Rules** guide.
3. **Protecting SSH?** Implement **Fail2Ban** to prevent brute-force attacks.

##  Contributing
- Found a security improvement? Open a **GitHub Issue** or submit a **Pull Request**.
- Want to enhance security? Suggest or add best practices!

---

This documentation ensures that all components are properly **hardened and secured** against threats.
