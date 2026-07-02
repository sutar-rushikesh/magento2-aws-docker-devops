# Phase 04 — Docker Compose Installation

## Objective

Install and verify Docker Compose Plugin on Ubuntu to orchestrate multiple Docker containers using a single `docker-compose.yml` file.

---

# What is Docker Compose?

Docker Compose is a tool used to define and manage multi-container Docker applications using a YAML configuration file.

Instead of running individual `docker run` commands, Docker Compose allows all services to be managed together.

Example services used in this project:

- Nginx
- PHP-FPM
- MySQL
- Redis
- OpenSearch

---

# Prerequisites

- Ubuntu Server
- Docker Engine Installed
- Internet Connectivity

---

# Verify Docker Compose Installation

Since Docker Compose Plugin was installed along with Docker Engine, verify its installation.

```bash
docker compose version
```

Expected Output

```
Docker Compose version v2.x.x
```

---

# Difference Between Old and New Compose

Old Version

```bash
docker-compose up -d
```

New Version (Recommended)

```bash
docker compose up -d
```

Docker now officially recommends using the new plugin syntax.

---

# Verify Docker Compose Plugin

```bash
docker compose version
```

Example

```
Docker Compose version v2.39.1
```

---

# Check Docker Compose Help

```bash
docker compose --help
```

This displays all available Docker Compose commands.

---

# Common Docker Compose Commands

Start all containers

```bash
docker compose up -d
```

View running containers

```bash
docker compose ps
```

View logs

```bash
docker compose logs
```

Follow logs

```bash
docker compose logs -f
```

Restart services

```bash
docker compose restart
```

Stop services

```bash
docker compose stop
```

Start stopped services

```bash
docker compose start
```

Remove containers

```bash
docker compose down
```

Remove containers, volumes, and networks

```bash
docker compose down -v
```

Pull latest images

```bash
docker compose pull
```

Build images

```bash
docker compose build
```

Rebuild and start

```bash
docker compose up -d --build
```

Execute command inside container

```bash
docker compose exec php bash
```

---

# Docker Compose File Structure

Docker Compose reads the following file by default:

```
docker-compose.yml
```

Example

```yaml
services:
  nginx:
    image: nginx

  php:
    image: php

  mysql:
    image: mysql
```

---

# Verify Installation

Run

```bash
docker compose version
```

Run

```bash
docker compose ls
```

Expected Output

```
NAME
```

If no projects are running, the list will be empty.

---

# Validation Checklist

| Check | Status |
|--------|--------|
| Docker Installed | ✅ |
| Docker Compose Plugin Installed | ✅ |
| Compose Version Verified | ✅ |
| Compose Commands Working | ✅ |

---

# Useful Commands

Show Compose Version

```bash
docker compose version
```

List Projects

```bash
docker compose ls
```

List Containers

```bash
docker compose ps
```

Show Logs

```bash
docker compose logs
```

Restart Stack

```bash
docker compose restart
```

Shutdown Stack

```bash
docker compose down
```

---

# Screenshots

Add the following screenshots.

```
screenshots/

docker-compose-version.png

docker-compose-help.png

docker-compose-ls.png
```

Example

```markdown
## Docker Compose Version

![Compose Version](../screenshots/docker-compose-version.png)

---

## Docker Compose Help

![Compose Help](../screenshots/docker-compose-help.png)

---

## Docker Compose Projects

![Compose LS](../screenshots/docker-compose-ls.png)
```

---

# Outcome

✔ Docker Compose Plugin verified

✔ Multi-container orchestration ready

✔ Server prepared for Magento Docker stack deployment

---

# Next Phase

In **Phase 05**, we will create the Magento project directory structure, organize configuration files, prepare the `.env` file, and understand the purpose of each folder before building the complete Docker Compose stack.
