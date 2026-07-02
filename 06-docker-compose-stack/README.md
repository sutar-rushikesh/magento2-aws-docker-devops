
# 🐳 Phase 06 — Docker Compose Infrastructure (Part 1)

## 🎯 Objective

In this phase, we build the complete Docker Compose infrastructure required to run Magento 2 in a production-ready environment.

Instead of manually installing every software package on the EC2 instance, Docker Compose allows us to run every component inside isolated containers.

By the end of this phase, we'll understand:

- Docker Compose architecture
- Multi-container applications
- Docker networking
- Docker volumes
- Environment variables
- Service communication
- Container dependencies

---

# 📚 Why Docker Compose?

Magento requires multiple services to function properly.

A typical Magento production environment consists of:

- Web Server
- PHP Runtime
- Database
- Cache
- Search Engine

Without Docker Compose, we would have to manually install and configure each component on the server.

Docker Compose simplifies this process by defining the entire infrastructure in a single YAML file.

---

# 🏗 Architecture

```
                    Internet
                         │
                  HTTPS (443)
                         │
                    Nginx Container
                         │
                  PHP-FPM Container
          ┌──────────┼──────────┐
          │          │          │
      MySQL      Redis    OpenSearch
```

---

# 📦 Containers Used

| Container | Purpose |
|------------|----------|
| Nginx | Reverse Proxy / Web Server |
| PHP-FPM | Executes Magento PHP Code |
| MySQL | Stores Magento Database |
| Redis | Cache & Session Storage |
| OpenSearch | Product Search Engine |

---

# 📂 Docker Compose File

The complete infrastructure is managed using a single file.

```
docker-compose.yml
```

This file defines:

- Services
- Networks
- Volumes
- Environment Variables
- Dependencies
- Restart Policies

---

# 📖 Understanding Docker Compose

Docker Compose allows multiple containers to work together as a single application.

Instead of starting containers individually:

```
docker run nginx

docker run mysql

docker run redis

docker run php
```

We simply execute:

```bash
docker compose up -d
```

Docker Compose automatically creates:

- Containers
- Networks
- Volumes

and connects everything together.

---

# 📁 Project Structure

```
magento-stack/
│
├── docker-compose.yml
├── .env
│
├── nginx/
│
├── php/
│
├── magento/
│
├── docs/
│
├── scripts/
│
└── screenshots/
```

---

# 🌐 Docker Network

All containers communicate through a dedicated bridge network.

Example:

```
magento-net
```

Instead of using IP addresses,

containers communicate using their service names.

Example:

```
Nginx
   │
   ▼
php:9000
```

PHP communicates with

```
mysql
```

instead of

```
172.xx.xx.xx
```

This makes the infrastructure portable and reliable.

---

# Why Use a Custom Network?

Advantages:

- Service Discovery
- Better Isolation
- Secure Communication
- No Hardcoded IP Addresses
- Easier Troubleshooting

---

# Network Configuration

```yaml
networks:

  magento-net:

    driver: bridge
```

---

# Docker Volumes

Containers are temporary.

If a container is deleted,

all internal data is lost.

Volumes solve this problem.

---

## Why Volumes?

Volumes store data outside containers.

Even if a container is recreated,

the data remains.

---

# Volumes Used

```
mysql_data
```

Stores

- Magento Database

---

```
opensearch_data
```

Stores

- Search Indexes

---

# Volume Configuration

```yaml
volumes:

  mysql_data:

  opensearch_data:
```

---

# Environment Variables

Instead of hardcoding configuration values,

Docker Compose reads them from

```
.env
```

---

## Benefits

✔ Easy configuration

✔ Secure

✔ Reusable

✔ Different environments

✔ No hardcoded passwords

---

# Example .env File

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

# Why Use .env?

Without environment variables

you would write

```yaml
image: mysql:8.0
```

With variables

```yaml
image: mysql:${MYSQL_VERSION}
```

Changing versions now requires editing only one file.

---

# Docker Compose Sections

Every compose file consists of:

```
services
```

Defines containers

---

```
networks
```

Defines communication

---

```
volumes
```

Defines persistent storage

---

Example

```yaml
services:

networks:

volumes:
```

---

# Service Communication

Docker automatically creates DNS entries.

Example

Nginx connects to PHP

```
php:9000
```

PHP connects to MySQL

```
mysql
```

PHP connects to Redis

```
redis
```

PHP connects to OpenSearch

```
opensearch
```

No IP addresses are required.

---

# Restart Policy

Every production container should restart automatically.

Example

```yaml
restart: unless-stopped
```

Benefits

- Recover from crashes
- Recover after reboot
- High availability

---

# Container Dependencies

Some containers require others to start first.

Example

PHP requires

- MySQL
- Redis
- OpenSearch

Compose handles this using

```yaml
depends_on
```

Example

```yaml
depends_on:
  - mysql
  - redis
  - opensearch
```

---

# Bringing Up the Stack

Once the compose file is ready,

start everything.

```bash
docker compose up -d
```

---

# Verify Running Containers

```bash
docker ps
```

Expected Output

```
magento-nginx

magento-php

magento-mysql

magento-redis

magento-opensearch
```

---

# Verify Docker Network

```bash
docker network ls
```

Inspect network

```bash
docker network inspect magento-stack_magento-net
```

---

# Verify Docker Volumes

```bash
docker volume ls
```

Expected

```
mysql_data

opensearch_data
```

---

# Validation Checklist

| Task | Status |
|------|--------|
| Docker Compose created | ✅ |
| Network created | ✅ |
| Volumes created | ✅ |
| Environment variables configured | ✅ |
| Containers communicate using service names | ✅ |
| Restart policies configured | ✅ |
| Dependencies configured | ✅ |

---

# 📸 Screenshots

Take the following screenshots.

---

## Docker Compose File

```
screenshots/docker-compose-overview.png
```

Markdown

```markdown
![Docker Compose](../screenshots/docker-compose-overview.png)
```

---

## Docker Network

```
screenshots/docker-network.png
```

Markdown

```markdown
![Docker Network](../screenshots/docker-network.png)
```

---

## Docker Volume

```
screenshots/docker-volumes.png
```

Markdown

```markdown
![Docker Volumes](../screenshots/docker-volumes.png)
```

---

## Running Containers

```
screenshots/docker-ps.png
```

Markdown

```markdown
![Docker Containers](../screenshots/docker-ps.png)
```

---

# 🎓 What You Learned

In this phase you learned:

- Docker Compose Fundamentals
- Multi-container Applications
- Docker Networks
- Docker Volumes
- Environment Variables
- Container Dependencies
- Service Discovery
- Production Deployment Concepts

---

# 🚀 Next Phase

In **Phase 06 – Part 2**, we will configure each service individually.

Topics include:

- Nginx Container
- PHP-FPM Container
- MySQL Container
- Redis Container
- OpenSearch Container
- Restart Policies
- Resource Optimization
- Health Checks


# 🐳 Phase 06 — Docker Compose Infrastructure (Part 2)

## 🎯 Objective

In the previous phase, we learned the fundamentals of Docker Compose.

In this phase, we will understand every service defined inside our `docker-compose.yml` file.

Instead of simply copying configuration from the internet, we'll understand:

- Why each container is required
- How containers communicate
- Why every configuration exists
- Production best practices

---

# Magento Container Architecture

```
                   Internet
                        │
                        │
                 HTTPS (443)
                        │
                 ┌──────────────┐
                 │    Nginx     │
                 └──────┬───────┘
                        │
                  FastCGI :9000
                        │
                 ┌──────────────┐
                 │   PHP-FPM    │
                 └──────┬───────┘
                        │
        ┌───────────────┼────────────────┐
        │               │                │
    MySQL           Redis          OpenSearch
```

---

# Docker Compose Overview

Our compose file contains five services.

| Service | Purpose |
|----------|----------|
| Nginx | Web Server |
| PHP-FPM | Executes Magento |
| MySQL | Database |
| Redis | Cache |
| OpenSearch | Product Search |

Each service runs inside its own isolated container.

---

# Service 1 — Nginx

Nginx acts as the frontend web server.

Responsibilities include

- Serving static files
- SSL termination
- Reverse proxy
- Sending PHP requests to PHP-FPM
- Delivering CSS, JavaScript and Images

Container Name

```
magento-nginx
```

Image

```yaml
image: nginx:alpine
```

Why Alpine?

- Lightweight
- Fast startup
- Small image size
- Production ready

---

## Port Mapping

```yaml
ports:

  - "80:80"

  - "443:443"
```

Meaning

Host Port

↓

Container Port

```
EC2:80
    │
    ▼
Nginx:80
```

```
EC2:443
    │
    ▼
Nginx:443
```

---

## Volumes

```yaml
volumes:

- ./magento:/var/www/html

- ./nginx/default.conf:/etc/nginx/conf.d/default.conf

- /etc/letsencrypt:/etc/letsencrypt:ro
```

Purpose

Magento Code

↓

Available inside container

Custom Nginx Configuration

↓

Overrides default configuration

SSL Certificates

↓

Read-only access

---

## depends_on

```yaml
depends_on:

- php
```

Meaning

Nginx starts after PHP container.

---

# Service 2 — PHP-FPM

PHP executes Magento application.

Container

```
magento-php
```

Build

```yaml
build:

 context: ./php
```

Instead of downloading a ready-made image,

we build our own custom PHP image.

Advantages

- Install Magento Extensions
- Configure PHP
- Install Composer
- Install required libraries

---

## PHP Volumes

```yaml
volumes:

- ./magento:/var/www/html

- ./php/conf.d/magento.ini:/usr/local/etc/php/conf.d/magento.ini
```

Magento Source Code

↓

Shared with PHP

PHP Configuration

↓

Custom php.ini values

---

## Restart Policy

```yaml
restart: unless-stopped
```

Production Best Practice

Container automatically starts

after

- reboot
- crash
- docker restart

---

## PHP Dependencies

```yaml
depends_on:

- mysql

- redis

- opensearch
```

PHP cannot function until these services are available.

---

# Service 3 — MySQL

Magento stores all business data inside MySQL.

Container

```
magento-mysql
```

Image

```yaml
image: mysql:8.0
```

---

## Environment Variables

```yaml
MYSQL_DATABASE

MYSQL_USER

MYSQL_PASSWORD

MYSQL_ROOT_PASSWORD
```

Docker initializes the database automatically.

---

## MySQL Command

```yaml
command:

--innodb-buffer-pool-size=128M

--max_connections=50
```

Purpose

Limit memory usage

Suitable for small EC2 instances

---

## Volume

```yaml
mysql_data
```

Without volume

Deleting container

↓

Entire database lost

With volume

Database survives container recreation.

---

# Service 4 — Redis

Redis provides

- Cache
- Session Storage
- Performance Improvement

Container

```
magento-redis
```

---

## Configuration

```yaml
command:

redis-server

--maxmemory 256mb

--maxmemory-policy allkeys-lru
```

Meaning

Redis uses

Maximum

```
256 MB
```

When full

↓

Least Recently Used Keys

↓

Automatically removed

Excellent for cache workloads.

---

# Service 5 — OpenSearch

Magento uses OpenSearch for

- Product Search
- Filters
- Search Suggestions
- Category Search

Container

```
magento-opensearch
```

---

## Environment Variables

```yaml
discovery.type=single-node
```

Used because

Only one OpenSearch node

runs in this project.

---

```yaml
DISABLE_SECURITY_PLUGIN=true
```

Disables authentication

Useful for development

and learning projects.

---

## Java Memory

```yaml
OPENSEARCH_JAVA_OPTS

-Xms256m

-Xmx256m
```

Limits JVM memory usage.

Ideal for

t2.small

t3.small

t3.medium

instances.

---

## Volume

```
opensearch_data
```

Stores

Search indexes

---

# Docker Network

All services join

```
magento-net
```

Example

PHP connects

```
mysql
```

instead of

```
172.18.0.7
```

Docker DNS automatically resolves

service names.

---

# Container Communication

```
Nginx

↓

php:9000

↓

Magento

↓

mysql

↓

redis

↓

opensearch
```

No IP addresses required.

---

# Restart Policies

Every container

```
restart: unless-stopped
```

Benefits

✔ Automatic restart

✔ High availability

✔ Crash recovery

✔ Server reboot recovery

---

# Docker Compose Lifecycle

Start

```bash
docker compose up -d
```

Stop

```bash
docker compose down
```

Restart

```bash
docker compose restart
```

Logs

```bash
docker compose logs
```

Status

```bash
docker ps
```

---

# Verify Services

Check Containers

```bash
docker ps
```

Expected

```
magento-nginx

magento-php

magento-mysql

magento-redis

magento-opensearch
```

---

Check Networks

```bash
docker network ls
```

---

Inspect Network

```bash
docker network inspect magento-stack_magento-net
```

---

Inspect Volumes

```bash
docker volume ls
```

---

Container Logs

```bash
docker logs magento-nginx

docker logs magento-php

docker logs magento-mysql
```

---

# Best Practices Followed

✅ Named Containers

✅ Custom Network

✅ Persistent Volumes

✅ Environment Variables

✅ Restart Policies

✅ Lightweight Images

✅ Service Dependencies

✅ Production-ready Structure

---

# 📸 Screenshots

Take screenshots of the following.

---

## docker ps

```
screenshots/docker-ps.png
```

```markdown
![Docker PS](../screenshots/docker-ps.png)
```

---

## Docker Network

```
screenshots/docker-network-inspect.png
```

```markdown
![Docker Network](../screenshots/docker-network-inspect.png)
```

---

## Docker Volumes

```
screenshots/docker-volume-list.png
```

```markdown
![Docker Volumes](../screenshots/docker-volume-list.png)
```

---

## docker compose up

```
screenshots/docker-compose-up.png
```

```markdown
![Docker Compose Up](../screenshots/docker-compose-up.png)
```

---

## Container Logs

```
screenshots/container-logs.png
```

```markdown
![Container Logs](../screenshots/container-logs.png)
```

---

# 🎓 What You Learned

By completing this phase, you now understand:

- Every Docker Compose service
- Nginx container
- PHP-FPM container
- MySQL container
- Redis container
- OpenSearch container
- Docker networking
- Docker volumes
- Restart policies
- Service dependencies
- Production container architecture

---

# 🚀 Next Phase

In **Phase 06 – Part 3**, we will build the final production-ready `docker-compose.yml`, explain every directive line-by-line, and cover validation, troubleshooting, optimization, and deployment verification.



# 🐳 Phase 06 — Docker Compose Infrastructure (Part 3)

## 🎯 Objective

In the previous parts, we learned:

- Docker Compose fundamentals
- Docker Compose services
- Container networking
- Persistent volumes
- Service communication

In this phase, we will deploy the complete Magento Docker stack using a production-ready `docker-compose.yml`, verify every container, validate networking, and troubleshoot common deployment issues.

---

# Final Architecture

```

                    Internet
                         │
                  HTTPS (443)
                         │
                 ┌───────────────┐
                 │    Nginx      │
                 └───────┬───────┘
                         │
                  FastCGI :9000
                         │
                 ┌───────────────┐
                 │   PHP-FPM     │
                 └───────┬───────┘
                         │
        ┌────────────────┼─────────────────┐
        │                │                 │
     MySQL            Redis          OpenSearch

```

---

# Production docker-compose.yml

Project Structure

```

magento-stack/

├── docker-compose.yml

├── .env

├── nginx/

├── php/

├── magento/

└── screenshots/

```

---

# Starting the Stack

Navigate to project directory

```bash
cd ~/magento-stack
```

Start all services

```bash
docker compose up -d
```

Expected Output

```
Creating network...

Creating magento-mysql

Creating magento-redis

Creating magento-opensearch

Creating magento-php

Creating magento-nginx
```

---

# Verify Running Containers

```bash
docker ps
```

Expected

```
CONTAINER ID

magento-nginx

magento-php

magento-mysql

magento-redis

magento-opensearch
```

---

# Verify Container Status

```bash
docker compose ps
```

Every service should display

```
State : Running
```

---

# Verify Docker Network

```bash
docker network ls
```

Expected

```
magento-stack_magento-net
```

Inspect network

```bash
docker network inspect magento-stack_magento-net
```

Verify

- nginx
- php
- mysql
- redis
- opensearch

are connected.

---

# Verify Docker Volumes

```bash
docker volume ls
```

Expected

```
mysql_data

opensearch_data
```

Volumes ensure data persists even after container deletion.

---

# Verify Container Communication

Open Nginx container

```bash
docker exec -it magento-nginx sh
```

Ping PHP

```bash
ping php
```

Expected

```
64 bytes from php
```

Exit

```bash
exit
```

---

# Verify PHP Container

```bash
docker exec -it magento-php php -v
```

Expected

```
PHP 8.3.x
```

---

Check Magento CLI

```bash
docker exec -it magento-php php bin/magento
```

Magento commands should appear.

---

# Verify Production Mode

```bash
docker exec -it magento-php php bin/magento deploy:mode:show
```

Expected

```
Current application mode:

production
```

---

# Verify Static Files

Check deployed assets

```bash
docker exec -it magento-php find pub/static/frontend | head
```

Expected

```
Magento

luma

styles-m.css

styles-l.css
```

---

# Verify CSS

```bash
curl -I https://yourdomain.com/static/frontend/Magento/luma/en_US/css/styles-m.css
```

Expected

```
HTTP/2 200
```

---

# Verify Home Page

```bash
curl -I https://yourdomain.com
```

Expected

```
HTTP/2 200
```

---

# Verify SSL

```bash
curl -Iv https://yourdomain.com
```

Expected

```
SSL certificate verify ok
```

---

# Verify Magento Services

Database

```bash
docker exec -it magento-php php bin/magento setup:db:status
```

Expected

```
All modules are up to date
```

---

Indexers

```bash
docker exec -it magento-php php bin/magento indexer:status
```

Expected

```
Ready
```

---

Cache

```bash
docker exec -it magento-php php bin/magento cache:status
```

Expected

```
All cache enabled
```

---

# Useful Docker Commands

Start

```bash
docker compose up -d
```

Stop

```bash
docker compose down
```

Restart

```bash
docker compose restart
```

Restart Single Container

```bash
docker restart magento-nginx
```

---

View Logs

Nginx

```bash
docker logs magento-nginx
```

PHP

```bash
docker logs magento-php
```

MySQL

```bash
docker logs magento-mysql
```

Redis

```bash
docker logs magento-redis
```

OpenSearch

```bash
docker logs magento-opensearch
```

---

# Troubleshooting

## 502 Bad Gateway

Cause

- PHP container stopped
- FastCGI configuration incorrect
- PHP-FPM not listening

Check

```bash
docker logs magento-nginx
```

---

## CSS Not Loading

Check

```bash
php bin/magento deploy:mode:show
```

Redeploy

```bash
php bin/magento setup:static-content:deploy -f
```

Flush cache

```bash
php bin/magento cache:flush
```

---

## Database Connection Error

Check

```bash
docker logs magento-mysql
```

Verify

```
MYSQL_DATABASE

MYSQL_USER

MYSQL_PASSWORD
```

---

## OpenSearch Connection Error

Check

```bash
docker logs magento-opensearch
```

Verify

```
DISABLE_SECURITY_PLUGIN=true
```

---

## Redis Error

Restart Redis

```bash
docker restart magento-redis
```

Verify connection

```bash
php bin/magento cache:status
```

---

# Performance Optimizations

Implemented

✅ Production Mode

✅ Redis Cache

✅ OpenSearch

✅ Persistent Volumes

✅ Lightweight Alpine Images

✅ Docker Bridge Network

✅ HTTPS

✅ Restart Policies

✅ Optimized PHP Configuration

---

# Validation Checklist

| Validation | Status |
|------------|--------|
| Docker Installed | ✅ |
| Compose Installed | ✅ |
| Containers Running | ✅ |
| Network Created | ✅ |
| Volumes Created | ✅ |
| Magento Accessible | ✅ |
| HTTPS Working | ✅ |
| Static Content Working | ✅ |
| PHP-FPM Connected | ✅ |
| Redis Connected | ✅ |
| MySQL Connected | ✅ |
| OpenSearch Connected | ✅ |

---

# Screenshots

Capture the following screenshots.

---

## Docker Compose Up

```
screenshots/docker-compose-up.png
```

```markdown
![Docker Compose Up](../screenshots/docker-compose-up.png)
```

---

## Docker PS

```
screenshots/docker-ps.png
```

```markdown
![Docker PS](../screenshots/docker-ps.png)
```

---

## Docker Network

```
screenshots/docker-network.png
```

```markdown
![Docker Network](../screenshots/docker-network.png)
```

---

## Docker Volumes

```
screenshots/docker-volumes.png
```

```markdown
![Docker Volumes](../screenshots/docker-volumes.png)
```

---

## Magento Homepage

```
screenshots/magento-homepage.png
```

```markdown
![Magento Homepage](../screenshots/magento-homepage.png)
```

---

## HTTPS

```
screenshots/https-working.png
```

```markdown
![HTTPS](../screenshots/https-working.png)
```

---

## Static Content

```
screenshots/static-content.png
```

```markdown
![Static Content](../screenshots/static-content.png)
```

---

## Container Logs

```
screenshots/container-logs.png
```

```markdown
![Container Logs](../screenshots/container-logs.png)
```

---

# What You Learned

By completing this phase, you can now:

- Deploy a multi-container Magento stack
- Build a production-ready Docker Compose environment
- Configure service dependencies
- Manage Docker volumes
- Configure Docker networking
- Verify inter-container communication
- Validate Magento production deployment
- Debug container issues
- Troubleshoot Nginx, PHP, Redis, MySQL and OpenSearch
- Monitor container health
- Deploy Magento using industry-standard Docker practices

---

# Final Outcome

At the end of Phase 06, you have successfully deployed a complete production-ready Magento 2 environment using Docker Compose consisting of:

- Nginx
- PHP-FPM
- MySQL
- Redis
- OpenSearch
- Docker Networks
- Docker Volumes
- HTTPS
- Magento Production Mode

This forms the foundation for the remaining phases, where we will configure Magento, enable SSL, optimize performance, and automate deployments using CI/CD.

---

# 🚀 Next Phase

**Phase 07 — Building the Custom PHP-FPM Docker Image**

In the next phase, we will create our own production-ready PHP Docker image by:

- Writing a custom `Dockerfile`
- Installing PHP extensions required by Magento
- Installing Composer
- Configuring PHP settings
- Optimizing image size
- Following Docker image best practices
- Building and testing the custom image
...
