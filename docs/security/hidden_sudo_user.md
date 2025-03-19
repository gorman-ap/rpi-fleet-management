# Hidden Sudo User Setup

## Overview
A hidden user has been created to manage system services securely without exposing administrative access to default accounts.

## User Details
- **Username:** (Hidden for security reasons)
- **Permissions:** Full `sudo` access
- **Purpose:** Runs all monitoring, logging, and automation services
- **Visibility:** The user does not appear in standard system login lists

## User Creation Steps

###  Create the pi-admin User
Since we want one user for all monitoring & logging services, create `pi-admin`:
```bash
sudo useradd -m -s /bin/bash pi-admin
```
- `-m` → Creates a home directory (`/home/pi-admin`).
- `-s /bin/bash` → Allows login if needed.

###  Set a Password for pi-admin
```bash
sudo passwd pi-admin
```
(You'll be prompted to set a password.)

###  Give pi-admin Sudo Privileges
```bash
sudo usermod -aG sudo pi-admin
```
 This allows `pi-admin` to run commands as root when needed.

## Security Considerations
- This account is only used for running services and administration.
- Regular user accounts do not have access to modify or interact with this user.
- Password authentication is disabled; only key-based access is allowed.
