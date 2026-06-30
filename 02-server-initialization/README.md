# Phase 02 — Linux Server Initialization

## Objective

Prepare the Ubuntu server for hosting a production Magento 2 environment by updating packages, installing essential utilities, configuring the timezone, and validating system resources.

---

# Prerequisites

- AWS EC2 instance is running
- SSH access is available
- Ubuntu 24.04 LTS

---

# Verify Server Information

Check the operating system.

```bash
lsb_release -a
```

Example Output

```
Distributor ID: Ubuntu
Description: Ubuntu 24.04 LTS
Release: 24.04
Codename: noble
```

---

Check Kernel Version

```bash
uname -r
```

---

Check Hostname

```bash
hostnamectl
```

---

Check Logged-in User

```bash
whoami
```

Expected Output

```
admin
```

---

# Update System Packages

Refresh package metadata.

```bash
sudo apt update
```

Upgrade installed packages.

```bash
sudo apt upgrade -y
```

---

# Install Required Packages

```bash
sudo apt install -y \
curl \
wget \
git \
vim \
unzip \
zip \
ca-certificates \
software-properties-common \
apt-transport-https \
gnupg \
lsb-release \
net-tools \
htop \
tree
```

These utilities are required for Docker installation, Magento deployment, troubleshooting, and day-to-day server administration.

---

# Verify Internet Connectivity

```bash
ping -c 4 google.com
```

---

# Configure Timezone

Display current timezone.

```bash
timedatectl
```

List available timezones.

```bash
timedatectl list-timezones
```

Set timezone to India Standard Time.

```bash
sudo timedatectl set-timezone Asia/Kolkata
```

Verify.

```bash
timedatectl
```

---

# Verify Disk Space

```bash
df -h
```

---

# Verify Memory

```bash
free -h
```

---

# Verify CPU Information

```bash
lscpu
```

---

# Verify Network Interfaces

```bash
ip addr
```

---

# Verify Open Ports

```bash
sudo ss -tulpn
```

---

# Verify Package Installation

```bash
git --version
curl --version
vim --version
tree --version
```

---

# Expected Result

The server should now have:

- Latest Ubuntu packages
- Required administration tools
- Correct timezone
- Working internet connectivity
- Verified CPU, RAM, and Disk

---

# Validation Checklist

| Check | Status |
|--------|--------|
| Ubuntu Updated | ✅ |
| Packages Installed | ✅ |
| Internet Working | ✅ |
| Timezone Configured | ✅ |
| Disk Verified | ✅ |
| Memory Verified | ✅ |
| CPU Verified | ✅ |

---

# Screenshots

Add the following screenshots to the `screenshots` folder.

```
screenshots/

ubuntu-version.png

apt-update.png

package-installation.png

timedatectl.png

disk-space.png

memory.png

cpu-info.png
```

Example

```markdown
## Ubuntu Version

![Ubuntu](../screenshots/ubuntu-version.png)

---

## System Update

![Update](../screenshots/apt-update.png)

---

## Installed Packages

![Packages](../screenshots/package-installation.png)

---

## Timezone

![Timezone](../screenshots/timedatectl.png)

---

## Disk Space

![Disk](../screenshots/disk-space.png)

---

## Memory

![Memory](../screenshots/memory.png)

---

## CPU Information

![CPU](../screenshots/cpu-info.png)
```

---

# Outcome

✔ Ubuntu server initialized

✔ Essential packages installed

✔ Server validated

✔ Ready for Docker installation
