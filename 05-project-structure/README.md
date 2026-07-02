# Phase 05 — Project Structure

## Objective

In this phase, we create a clean, scalable, and production-ready directory structure for the Magento 2 Docker deployment.

A well-organized project structure makes configuration management, troubleshooting, collaboration, and future enhancements much easier.

---

# Why Proper Project Structure?

A standardized project layout helps us:

- Organize configuration files
- Separate application code from infrastructure
- Improve maintainability
- Simplify troubleshooting
- Support Git version control
- Prepare for CI/CD pipelines
- Follow DevOps best practices

---

# Final Project Structure

```text
magento-stack/
│
├── docker-compose.yml
├── .env
├── README.md
│
├── magento/
│
├── nginx/
│   └── default.conf
│
├── php/
│   ├── Dockerfile
│   └── conf.d/
│       └── magento.ini
│
├── scripts/
│
├── docs/
│
└── screenshots/
```

---

# Step 1 — Create Project Directory

```bash
mkdir magento-stack
cd magento-stack
```

---

# Step 2 — Create Required Directories

```bash
mkdir -p \
magento \
nginx \
php/conf.d \
scripts \
docs \
screenshots
```

---

# Step 3 — Create Required Files

```bash
touch \
docker-compose.yml \
.env \
README.md \
nginx/default.conf \
php/Dockerfile \
php/conf.d/magento.ini
```

---

# Project Structure Overview

## Root Directory

Contains the main project configuration files.

```text
docker-compose.yml
.env
README.md
```

---

## Magento Directory

```text
magento/
```

Purpose:

- Magento application source code
- Vendor packages
- Generated files
- Static assets
- Media files

---

## Nginx Directory

```text
nginx/
└── default.conf
```

Purpose:

- Nginx Virtual Host Configuration
- PHP Routing
- Static File Handling
- SSL Configuration
- Security Rules

---

## PHP Directory

```text
php/
├── Dockerfile
└── conf.d/
```

Purpose:

- Build custom PHP image
- Install PHP extensions
- Configure PHP-FPM
- Customize PHP settings

---

## PHP Configuration

```text
php/conf.d/

magento.ini
```

Example settings

- memory_limit
- upload_max_filesize
- max_execution_time
- post_max_size

---

## Scripts Directory

```text
scripts/
```

Purpose

Automation scripts such as:

- Backup scripts
- Restore scripts
- Deployment scripts
- Cleanup scripts

Example

```text
scripts/
├── backup.sh
├── restore.sh
└── deploy.sh
```

---

## Documentation Directory

```text
docs/
```

Purpose

Additional project documentation

Example

```text
docs/
├── Architecture.md
├── Notes.md
└── Troubleshooting.md
```

---

## Screenshots Directory

```text
screenshots/
```

Purpose

Store screenshots used inside README files.

Example

```text
screenshots/
├── project-structure.png
├── docker-compose.png
├── nginx.png
└── magento-homepage.png
```

---

# Environment File

Create

```text
.env
```

Example

```env
MYSQL_VERSION=8.0

MYSQL_DATABASE=magento

MYSQL_USER=magento

MYSQL_PASSWORD=password

MYSQL_ROOT_PASSWORD=rootpassword

REDIS_VERSION=7-alpine

OPENSEARCH_VERSION=2.19.1
```

---

# Verify Directory Structure

Install Tree

```bash
sudo apt update

sudo apt install tree -y
```

Run

```bash
tree -L 2
```

Expected Output

```text
.
├── docker-compose.yml
├── .env
├── README.md
├── magento
├── nginx
├── php
├── scripts
├── docs
└── screenshots
```

---

# Verify Files

```bash
find . -maxdepth 2
```

Example Output

```text
docker-compose.yml
.env
README.md
nginx/default.conf
php/Dockerfile
php/conf.d/magento.ini
```

---

# Best Practices Followed

- Infrastructure separated from application
- Environment variables stored in `.env`
- Nginx configuration isolated
- PHP configuration isolated
- Scripts stored separately
- Documentation centralized
- Screenshots organized

---

# Validation Checklist

| Item | Status |
|------|--------|
| Project directory created | ✅ |
| Magento directory created | ✅ |
| Nginx directory created | ✅ |
| PHP directory created | ✅ |
| Dockerfile created | ✅ |
| PHP configuration created | ✅ |
| Docker Compose file created | ✅ |
| Environment file created | ✅ |
| Screenshots directory created | ✅ |
| Tree command verified | ✅ |

---

# Screenshots

Capture the following screenshots.

## Project Directory

```text
screenshots/project-directory.png
```

---

## Tree Output

```text
screenshots/tree-output.png
```

---

## Project Files

```text
screenshots/project-files.png
```

---

## Example

```markdown
## Project Directory

![Project Directory](../screenshots/project-directory.png)

---

## Tree Output

![Tree Output](../screenshots/tree-output.png)

---

## Project Files

![Project Files](../screenshots/project-files.png)
```

---

# Learning Outcomes

After completing this phase, you will understand:

- How to organize a real-world DevOps repository
- Why separating infrastructure and application code is important
- Directory layout for Dockerized Magento projects
- How to prepare the project for version control
- Best practices for scalable project organization

---

# Next Phase

In **Phase 06**, we will build the complete **Docker Compose stack**, including:

- Docker Networks
- Persistent Volumes
- Nginx Container
- PHP-FPM Container
- MySQL Container
- Redis Container
- OpenSearch Container
- Environment Variables
- Service Dependencies
- Container Health Verification

...
