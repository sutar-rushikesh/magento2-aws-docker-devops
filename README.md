# Magento 2 Production Deployment on AWS using Docker & Docker Compose

![AWS](https://img.shields.io/badge/AWS-EC2-orange)
![Docker](https://img.shields.io/badge/Docker-Compose-blue)
![Magento](https://img.shields.io/badge/Magento-2.4.x-red)
![Nginx](https://img.shields.io/badge/Nginx-Reverse%20Proxy-green)
![PHP](https://img.shields.io/badge/PHP-8.3-777BB4)
![MySQL](https://img.shields.io/badge/MySQL-8.0-blue)
![Redis](https://img.shields.io/badge/Redis-Cache-red)
![OpenSearch](https://img.shields.io/badge/OpenSearch-2.x-005EB8)
![SSL](https://img.shields.io/badge/HTTPS-Let's%20Encrypt-success)

---

# Project Overview

This repository contains the complete implementation guide for deploying **Magento 2 in a production-ready environment** on **AWS EC2** using **Docker Compose**.

The documentation is organized into multiple phases so anyone can reproduce the deployment from scratch.

This project demonstrates practical DevOps skills including:

- AWS EC2
- Linux Administration
- Docker
- Docker Compose
- Magento 2
- Nginx
- PHP-FPM
- MySQL
- Redis
- OpenSearch
- HTTPS using Let's Encrypt
- Domain Configuration
- Production Optimization
- Git & GitHub

---

# Live Demo

**Website**

https://pittylittle.shop

---

# Architecture

```
                    Internet
                         │
                  GoDaddy DNS
                         │
                  pittylittle.shop
                         │
                AWS EC2 (Ubuntu)
                         │
                Docker Compose Stack
                         │
     ┌────────────────────────────────────┐
     │                                    │
     │            Nginx                   │
     │               │                    │
     │          PHP-FPM                   │
     │               │                    │
     │          Magento 2                 │
     │               │                    │
     │ ┌────────┬────────┬────────────┐   │
     │ │ MySQL │ Redis  │ OpenSearch │   │
     │ └────────┴────────┴────────────┘   │
     └────────────────────────────────────┘
```

---

# Technology Stack

| Technology | Version |
|------------|----------|
| AWS EC2 | Ubuntu 24.04 |
| Docker | Latest |
| Docker Compose | Latest |
| Magento | 2.4.x |
| PHP | 8.3 |
| MySQL | 8 |
| Redis | Latest |
| OpenSearch | 2.x |
| Nginx | Alpine |
| SSL | Let's Encrypt |
| GitHub | Git |

---

# Implementation Phases

This repository is divided into multiple implementation phases.

| Phase | Description |
|--------|-------------|
| Phase 01 | AWS EC2 Setup |
| Phase 02 | Linux Server Initialization |
| Phase 03 | Docker Installation |
| Phase 04 | Docker Compose Installation |
| Phase 05 | Magento Project Setup |
| Phase 06 | Docker Compose Configuration |
| Phase 07 | PHP Image Configuration |
| Phase 08 | MySQL Configuration |
| Phase 09 | Redis Configuration |
| Phase 10 | OpenSearch Configuration |
| Phase 11 | Magento Installation |
| Phase 12 | Nginx Configuration |
| Phase 13 | Domain Configuration |
| Phase 14 | SSL using Let's Encrypt |
| Phase 15 | Production Optimization |
| Phase 16 | Static Content Deployment |
| Phase 17 | Magento Cron Jobs |
| Phase 18 | Troubleshooting |
| Phase 19 | Final Validation |
| Phase 20 | GitHub Deployment |

---

# Repository Structure

```
magento2-aws-docker-devops/

│
├── README.md
│
├── screenshots/
│
├── 01-aws-ec2-setup/
├── 02-linux-server-setup/
├── 03-docker-installation/
├── 04-docker-compose-installation/
├── 05-magento-source/
├── 06-docker-compose/
├── 07-php/
├── 08-mysql/
├── 09-redis/
├── 10-opensearch/
├── 11-magento-installation/
├── 12-nginx/
├── 13-domain-configuration/
├── 14-ssl/
├── 15-production-mode/
├── 16-static-content/
├── 17-cron/
├── 18-troubleshooting/
├── 19-final-validation/
└── 20-github/
```

---

# Screenshots

The project contains screenshots for every major implementation step.

Examples include:

- AWS EC2 Launch
- Security Groups
- Docker Installation
- Docker Compose
- Magento Installation
- Docker Containers
- Nginx Configuration
- SSL Certificate
- Production Mode
- Magento Homepage
- Admin Dashboard

---

# Skills Demonstrated

- AWS EC2
- Ubuntu Linux
- Docker
- Docker Compose
- Magento 2
- Nginx
- PHP-FPM
- MySQL
- Redis
- OpenSearch
- DNS
- HTTPS
- Let's Encrypt
- Linux Networking
- Git
- GitHub
- DevOps Best Practices

---

# Target Audience

This repository is useful for:

- DevOps Engineers
- Cloud Engineers
- Linux Administrators
- Magento Developers
- Docker Beginners
- Students preparing DevOps portfolios
- Interview Preparation

---

# Future Improvements

- Jenkins CI/CD Pipeline
- GitHub Actions
- Terraform Infrastructure
- Ansible Automation
- Kubernetes Deployment
- Helm Charts
- Monitoring using Prometheus
- Grafana Dashboards
- Loki Logging
- AWS Load Balancer
- Auto Scaling

---

# Author

**Rushikesh Sutar**

DevOps Engineer

GitHub:

https://github.com/sutar-rushikesh


---

# License

This project is created for educational and portfolio purposes.
