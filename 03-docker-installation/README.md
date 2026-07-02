# Phase 03 — Docker Installation

## Objective

Install the latest Docker Engine on Ubuntu using Docker's official APT repository and verify that Docker is functioning correctly.

---

# Why Install Docker from the Official Repository?

Although Ubuntu provides Docker packages through its default repositories, they are often behind the latest stable release.

Installing from Docker's official repository provides:

- Latest stable Docker Engine
- Latest Docker CLI
- Docker Buildx
- Docker Compose Plugin
- Security updates directly from Docker

---

# Prerequisites

- Ubuntu Server initialized
- Internet connectivity
- Sudo privileges

---

# Step 1 — Remove Old Docker Packages

```bash
sudo apt remove docker docker-engine docker.io containerd runc -y
```

If Docker has never been installed, this command will simply report that no packages were found.

---

# Step 2 — Update Package Index

```bash
sudo apt update
```

---

# Step 3 — Install Required Dependencies

```bash
sudo apt install -y \
ca-certificates \
curl \
gnupg
```

---

# Step 4 — Create Docker Keyring Directory

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

---

# Step 5 — Download Docker GPG Key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

Set the correct permissions:

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

---

# Step 6 — Add Docker Repository

```bash
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

# Step 7 — Update Package Index

```bash
sudo apt update
```

---

# Step 8 — Install Docker

```bash
sudo apt install -y \
docker-ce \
docker-ce-cli \
containerd.io \
docker-buildx-plugin \
docker-compose-plugin
```

---

# Step 9 — Verify Docker Service

Check the Docker service status.

```bash
sudo systemctl status docker
```

Expected Status

```
active (running)
```

Enable Docker to start on boot.

```bash
sudo systemctl enable docker
```

---

# Step 10 — Verify Docker Version

```bash
docker --version
```

Example

```
Docker version 28.x.x
```

---

# Step 11 — Verify Docker Compose

```bash
docker compose version
```

Example

```
Docker Compose version v2.x.x
```

---

# Step 12 — Verify Docker Information

```bash
docker info
```

This displays:

- Storage Driver
- Docker Root Directory
- CPUs
- Memory
- Running Containers
- Images

---

# Step 13 — Run Test Container

```bash
sudo docker run hello-world
```

Expected Output

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

---

# Step 14 — Allow Docker Without sudo (Optional)

Add your user to the Docker group.

```bash
sudo usermod -aG docker $USER
```

Apply the new group membership.

```bash
newgrp docker
```

Verify.

```bash
docker ps
```

---

# Validation Checklist

| Check | Status |
|--------|--------|
| Old Packages Removed | ✅ |
| Docker Repository Added | ✅ |
| Docker Installed | ✅ |
| Docker Service Running | ✅ |
| Docker Compose Installed | ✅ |
| Hello World Container Executed | ✅ |

---

# Useful Commands

Check Docker version

```bash
docker --version
```

Check Docker Compose version

```bash
docker compose version
```

List running containers

```bash
docker ps
```

List all containers

```bash
docker ps -a
```

List Docker images

```bash
docker images
```

Check Docker service

```bash
systemctl status docker
```

Restart Docker

```bash
sudo systemctl restart docker
```

---

# Screenshots

Add the following screenshots to the `screenshots` folder.

```
screenshots/

docker-version.png

docker-service.png

docker-compose-version.png

docker-info.png

hello-world.png
```

Example

```markdown
## Docker Version

![Docker Version](../screenshots/docker-version.png)

---

## Docker Service

![Docker Service](../screenshots/docker-service.png)

---

## Docker Compose

![Docker Compose](../screenshots/docker-compose-version.png)

---

## Docker Info

![Docker Info](../screenshots/docker-info.png)

---

## Hello World Container

![Hello World](../screenshots/hello-world.png)
```

---

# Outcome

✔ Docker Engine installed

✔ Docker Compose installed

✔ Docker service enabled

✔ Verified with Hello World container

✔ Server ready for Magento container deployment
