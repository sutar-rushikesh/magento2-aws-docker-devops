
# 🛒 Phase 09 — Installing Magento 2 Using Composer

## 🎯 Objective

In the previous phases, we successfully configured the complete Docker infrastructure required to run Magento 2.

Our stack now includes:

- Docker Compose
- Custom PHP-FPM Image
- Nginx
- MySQL
- Redis
- OpenSearch

In this phase, we will install Magento 2 using Composer and configure it to work with all supporting services.

By the end of this phase, you will have a fully functional Magento 2 storefront running in Docker.

---

# Architecture

```

                     Internet
                          │
                     HTTPS (443)
                          │
                    ┌────────────┐
                    │   Nginx    │
                    └─────┬──────┘
                          │
                   FastCGI Port 9000
                          │
                    ┌────────────┐
                    │ PHP-FPM    │
                    └─────┬──────┘
                          │
                  Magento Application
                          │
      ┌──────────┬────────────┬────────────┐
      │          │            │            │
   MySQL      Redis      OpenSearch   Composer

```

---

# Prerequisites

Before installing Magento, verify the following services are running.

```bash
docker ps
```

Expected containers:

- magento-nginx
- magento-php
- magento-mysql
- magento-redis
- magento-opensearch

---

# Verify PHP

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

# Verify MySQL

```bash
docker exec -it magento-mysql mysql -u root -p
```

---

# Verify Redis

```bash
docker exec -it magento-redis redis-cli ping
```

Expected

```
PONG
```

---

# Verify OpenSearch

```bash
curl http://localhost:9200
```

Expected

```
cluster_name
```

---

# Obtain Magento Marketplace Keys

Magento packages are private.

Create Marketplace Authentication Keys.

Visit

```
https://marketplace.magento.com/
```

Navigate to

```
My Profile

↓

Access Keys

↓

Create New Access Key
```

You will receive:

- Public Key
- Private Key

---

# Configure Composer Authentication

Inside the PHP container:

```bash
composer config --global http-basic.repo.magento.com PUBLIC_KEY PRIVATE_KEY
```

Verify

```bash
composer config --global --list
```

---

# Create Magento Project

Navigate to your web directory.

```bash
cd /var/www/html
```

Create project

```bash
composer create-project \
--repository-url=https://repo.magento.com/ \
magento/project-community-edition .
```

Composer downloads:

- Magento Core
- Vendor Packages
- Dependencies

---

# Verify Installation

List files

```bash
ls
```

Expected

```
app

bin

generated

lib

pub

setup

vendor

var
```

---

# Magento Installation Command

Run

```bash
php bin/magento setup:install \
--base-url=https://yourdomain.com \
--db-host=mysql \
--db-name=magento \
--db-user=magento \
--db-password=password \
--admin-firstname=Rushikesh \
--admin-lastname=Sutar \
--admin-email=admin@example.com \
--admin-user=admin \
--admin-password=Admin@123 \
--language=en_US \
--currency=USD \
--timezone=Asia/Kolkata \
--use-rewrites=1 \
--search-engine=opensearch \
--opensearch-host=opensearch \
--opensearch-port=9200 \
--cache-backend=redis \
--cache-backend-redis-server=redis \
--page-cache=redis \
--page-cache-redis-server=redis \
--session-save=redis \
--session-save-redis-host=redis
```

---

# Installation Parameters

| Parameter | Description |
|-----------|-------------|
| Base URL | Website URL |
| Database | MySQL |
| Redis | Cache |
| OpenSearch | Search Engine |
| Admin Credentials | Login Details |
| Timezone | Store Timezone |
| Currency | Store Currency |
| Language | Store Language |

---

# Installation Process

Magento performs

- Database creation
- Module registration
- Dependency Injection
- Cache initialization
- Admin user creation
- URL rewrite generation

Installation takes several minutes.

---

# Installation Complete

Expected output

```
[SUCCESS]

Magento installation complete.
```

---

# Admin URL

Magento generates

```
Admin URI:

xxxxxxxx
```

Save this URL.

---

# Verify Homepage

Open

```
https://yourdomain.com
```

Expected

Magento Luma Storefront

---

# Verify Admin Panel

Open

```
https://yourdomain.com/admin_xxxxxx
```

Login using

```
Admin Username

Admin Password
```

---

# Verify Magento Version

```bash
php bin/magento --version
```

Example

```
Magento CLI 2.4.x
```

---

# Verify Installed Modules

```bash
php bin/magento module:status
```

---

# Verify Database

```bash
php bin/magento setup:db:status
```

Expected

```
All modules are up to date.
```

---

# Verify Cache

```bash
php bin/magento cache:status
```

Expected

All cache types enabled.

---

# Verify Indexers

```bash
php bin/magento indexer:status
```

Expected

```
Ready
```

---

# Verify Production Mode

```bash
php bin/magento deploy:mode:show
```

Expected

```
production
```

---

# Useful Commands

Flush Cache

```bash
php bin/magento cache:flush
```

Compile

```bash
php bin/magento setup:di:compile
```

Deploy Static Content

```bash
php bin/magento setup:static-content:deploy -f
```

Reindex

```bash
php bin/magento indexer:reindex
```

Maintenance Mode

Enable

```bash
php bin/magento maintenance:enable
```

Disable

```bash
php bin/magento maintenance:disable
```

---

# Common Installation Errors

## Database Connection Failed

Verify

```bash
docker ps
```

Ensure MySQL is running.

---

## Redis Connection Failed

Verify

```bash
redis-cli ping
```

---

## OpenSearch Error

Verify

```bash
curl http://opensearch:9200
```

---

## Composer Authentication Failed

Check Marketplace Keys.

---

## Memory Limit Error

Increase

```
memory_limit = 2G
```

---

## Permission Error

Run

```bash
find var generated vendor pub/static pub/media -type f -exec chmod 664 {} \;

find var generated vendor pub/static pub/media -type d -exec chmod 775 {} \;
```

---

# Best Practices

✔ Composer Authentication

✔ Latest Magento Version

✔ Docker Installation

✔ Redis Integration

✔ OpenSearch Integration

✔ Production Configuration

✔ Secure Admin Credentials

✔ HTTPS

✔ Docker Networking

✔ Repeatable Installation

---

# Validation Checklist

| Validation | Status |
|------------|--------|
| Composer Installed | ✅ |
| Magento Downloaded | ✅ |
| Database Connected | ✅ |
| Redis Connected | ✅ |
| OpenSearch Connected | ✅ |
| Installation Successful | ✅ |
| Storefront Working | ✅ |
| Admin Working | ✅ |
| CLI Working | ✅ |
| Production Ready | ✅ |

---

# Screenshots

Capture the following screenshots.

---

## Composer Authentication

```
screenshots/composer-auth.png
```

```markdown
![Composer Authentication](../screenshots/composer-auth.png)
```

---

## Composer Installation

```
screenshots/composer-install.png
```

```markdown
![Composer Install](../screenshots/composer-install.png)
```

---

## Magento Setup Install

```
screenshots/setup-install.png
```

```markdown
![Magento Install](../screenshots/setup-install.png)
```

---

## Installation Success

```
screenshots/install-success.png
```

```markdown
![Installation Success](../screenshots/install-success.png)
```

---

## Magento Homepage

```
screenshots/magento-homepage.png
```

```markdown
![Homepage](../screenshots/magento-homepage.png)
```

---

## Magento Admin Login

```
screenshots/admin-login.png
```

```markdown
![Admin Login](../screenshots/admin-login.png)
```

---

## Magento Dashboard

```
screenshots/admin-dashboard.png
```

```markdown
![Admin Dashboard](../screenshots/admin-dashboard.png)
```

---

## Magento Version

```
screenshots/magento-version.png
```

```markdown
![Magento Version](../screenshots/magento-version.png)
```

---

## Cache Status

```
screenshots/cache-status.png
```

```markdown
![Cache Status](../screenshots/cache-status.png)
```

---

## Indexer Status

```
screenshots/indexer-status.png
```

```markdown
![Indexer Status](../screenshots/indexer-status.png)
```

---

# Skills Demonstrated

After completing this phase, you have demonstrated the ability to:

- Authenticate Composer with Adobe Commerce Marketplace
- Install Magento 2 using Composer
- Configure Magento with MySQL, Redis, and OpenSearch
- Execute the Magento installation process
- Verify the installation through CLI and browser
- Access and manage the Magento Admin Panel
- Troubleshoot common installation issues
- Prepare Magento for production deployment

---

# Final Outcome

At the end of this phase, you have successfully installed **Magento 2 Community Edition** using Composer within a Dockerized environment.

The application is fully integrated with MySQL, Redis, OpenSearch, PHP-FPM, and Nginx, and is accessible over HTTPS with both the storefront and the admin panel operational. This forms the foundation for all future configuration, optimization, and deployment activities.

---

# 🚀 Next Phase

**Phase 10 — Configuring Redis, OpenSearch, Production Mode & Post-Installation Optimization**

In the next phase, we will:

- Verify Redis integration
- Verify OpenSearch integration
- Configure Full Page Cache
- Enable Production Mode
- Deploy Static Content
- Compile Dependency Injection
- Configure Cron Jobs
- Optimize Magento for production performance
...
