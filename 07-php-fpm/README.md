
# 🐘 Phase 07 — Building a Custom PHP-FPM Docker Image for Magento 2

## 🎯 Objective

In previous phases, we deployed Magento 2 using Docker Compose and connected all required services.

However, instead of using the official PHP image directly, production environments typically require a **custom Docker image** with all Magento dependencies pre-installed.

In this phase, we will build a custom PHP-FPM image optimized specifically for Magento 2.

---

# Why Build a Custom PHP Image?

Using the default PHP image is not sufficient for Magento because it does not include many required extensions.

A custom image provides:

- Faster deployments
- Reproducible builds
- Easier maintenance
- Consistent environments
- Production optimization
- Better portability

---

# Final Architecture

```

                Docker Compose

                      │

             Custom PHP Docker Image

                      │

      ┌───────────────┴────────────────┐

      │                                │

Magento Source                  PHP Extensions

      │                                │

 Composer                    Required Libraries

      │                                │

      └───────────────┬────────────────┘

                  PHP-FPM 8.3

```

---

# Magento PHP Requirements

Magento requires several PHP extensions.

| Extension | Purpose |
|------------|----------|
| bcmath | Mathematical calculations |
| ctype | Character validation |
| curl | API communication |
| dom | XML parsing |
| gd | Image processing |
| intl | Internationalization |
| mbstring | UTF-8 support |
| mysqli | Database |
| pdo_mysql | MySQL driver |
| soap | SOAP APIs |
| simplexml | XML parsing |
| sockets | Socket communication |
| xsl | XML transformations |
| zip | Archive handling |
| opcache | Performance |
| exif | Image metadata |

---

# Project Structure

```

php/

├── Dockerfile

├── www.conf

├── conf.d/

│      └── magento.ini

└── entrypoint.sh

```

---

# Base Image

We use the official PHP-FPM image.

```
php:8.3-fpm
```

Why PHP-FPM?

- Lightweight
- High performance
- Production ready
- Works perfectly with Nginx

---

# Installing System Packages

Magento requires several Linux packages.

Example:

```dockerfile
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    zip \
    curl \
    wget \
    libicu-dev \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libxml2-dev \
    libxslt1-dev \
    libonig-dev \
    libcurl4-openssl-dev \
    libsodium-dev \
    libssl-dev
```

---

# Installing PHP Extensions

Required extensions are compiled using Docker.

Example

```dockerfile
docker-php-ext-install \
    bcmath \
    intl \
    pdo_mysql \
    soap \
    sockets \
    xsl \
    zip \
    opcache
```

---

# Installing GD Extension

Magento requires GD for image processing.

```dockerfile
docker-php-ext-configure gd \
    --with-freetype \
    --with-jpeg

docker-php-ext-install gd
```

---

# Installing Composer

Composer is required for Magento dependency management.

```dockerfile
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
```

Verify

```bash
composer --version
```

---

# PHP Configuration

Custom configuration file

```

php/conf.d/magento.ini

```

Example

```ini
memory_limit=2G

max_execution_time=1800

upload_max_filesize=64M

post_max_size=64M

date.timezone=UTC

opcache.enable=1

opcache.memory_consumption=512

opcache.max_accelerated_files=100000

realpath_cache_size=10M
```

---

# PHP-FPM Configuration

Customize

```

www.conf

```

Important parameters

```
pm = dynamic

pm.max_children = 20

pm.start_servers = 5

pm.min_spare_servers = 2

pm.max_spare_servers = 10
```

These settings improve PHP performance.

---

# Dockerfile Overview

The Dockerfile performs the following tasks:

- Pull PHP 8.3
- Install Linux dependencies
- Install PHP extensions
- Configure GD
- Install Composer
- Copy PHP configuration
- Configure PHP-FPM
- Set working directory
- Start PHP-FPM

---

# Building the Image

Build image

```bash
docker compose build php
```

or

```bash
docker build -t magento-php .
```

---

# Starting Container

```bash
docker compose up -d
```

---

# Verify PHP Version

```bash
docker exec -it magento-php php -v
```

Expected

```
PHP 8.3.x
```

---

# Verify Composer

```bash
docker exec -it magento-php composer --version
```

Expected

```
Composer version 2.x
```

---

# Verify PHP Extensions

```bash
docker exec -it magento-php php -m
```

Expected

```
bcmath

curl

dom

gd

intl

mbstring

mysqli

pdo_mysql

soap

sockets

xsl

zip

Zend OPcache
```

---

# Verify PHP Configuration

```bash
docker exec -it magento-php php -i
```

Check

```
memory_limit

upload_max_filesize

post_max_size

date.timezone
```

---

# Verify Magento CLI

```bash
docker exec -it magento-php php bin/magento
```

Magento commands should appear successfully.

---

# Verify Production Mode

```bash
docker exec -it magento-php php bin/magento deploy:mode:show
```

Expected

```
production
```

---

# Docker Image Inspection

List images

```bash
docker images
```

Expected

```
magento-php
```

Inspect image

```bash
docker inspect magento-php
```

---

# Best Practices Implemented

✔ Official PHP Base Image

✔ Multi-stage Build

✔ Composer from Official Image

✔ Production PHP Settings

✔ Lightweight Image

✔ Optimized Layers

✔ Docker Cache Friendly

✔ Separate Configuration Files

✔ No Unnecessary Packages

✔ Production Ready

---

# Troubleshooting

## Missing Extension

Check

```bash
php -m
```

Rebuild image

```bash
docker compose build --no-cache php
```

---

## Composer Missing

Verify

```bash
composer --version
```

Rebuild image.

---

## PHP-FPM Not Starting

Check

```bash
docker logs magento-php
```

---

## Magento CLI Not Working

Verify

```bash
php bin/magento
```

Check permissions

```bash
ls -la
```

---

# Validation Checklist

| Validation | Status |
|------------|--------|
| Dockerfile Created | ✅ |
| PHP Image Built | ✅ |
| Composer Installed | ✅ |
| PHP Extensions Installed | ✅ |
| PHP Configuration Applied | ✅ |
| PHP-FPM Running | ✅ |
| Magento CLI Working | ✅ |
| Production Ready | ✅ |

---

# Screenshots

Capture the following screenshots.

---

## Dockerfile

```
screenshots/dockerfile.png
```

```markdown
![Dockerfile](../screenshots/dockerfile.png)
```

---

## Docker Build

```
screenshots/docker-build.png
```

```markdown
![Docker Build](../screenshots/docker-build.png)
```

---

## Docker Images

```
screenshots/docker-images.png
```

```markdown
![Docker Images](../screenshots/docker-images.png)
```

---

## PHP Version

```
screenshots/php-version.png
```

```markdown
![PHP Version](../screenshots/php-version.png)
```

---

## Composer Version

```
screenshots/composer-version.png
```

```markdown
![Composer Version](../screenshots/composer-version.png)
```

---

## PHP Modules

```
screenshots/php-modules.png
```

```markdown
![PHP Modules](../screenshots/php-modules.png)
```

---

## Magento CLI

```
screenshots/magento-cli.png
```

```markdown
![Magento CLI](../screenshots/magento-cli.png)
```

---

# Skills Demonstrated

By completing this phase, you have learned:

- Docker Image Creation
- Dockerfile Best Practices
- Multi-stage Builds
- PHP-FPM Configuration
- Composer Installation
- Magento PHP Requirements
- Docker Layer Optimization
- Production PHP Configuration
- Docker Image Debugging
- Docker Build Process

---

# Final Outcome

At the end of this phase, you have successfully built a **custom production-ready PHP-FPM Docker image** tailored for Magento 2.

This image includes all required PHP extensions, Composer, optimized PHP settings, and production-ready configurations. It serves as the application runtime for the Magento Docker stack and provides a reusable, portable foundation for future deployments.

---

# 🚀 Next Phase

**Phase 08 — Configuring Nginx for Magento 2**

In the next phase, we will:

- Configure Nginx as a reverse proxy
- Integrate PHP-FPM
- Configure FastCGI
- Optimize static content delivery
- Secure the server
- Improve performance with caching headers
- Prepare the web server for production traffic
...
